

## Table des Matières
1. [Structure de Base](#1-structure-de-base)
2. [Compilation et Exécution](#2-compilation-et-exécution)
3. [Directives Assembleur](#3-directives-assembleur)
4. [Variables et Types de Données](#4-variables-et-types-de-données)
5. [Instructions de Base](#5-instructions-de-base)
6. [Contrôle de Flux](#6-contrôle-de-flux)
7. [Registres et Flags](#7-registres-et-flags)
8. [Optimisations](#8-optimisations)
9. [Différences x32/x64](#9-différences-x32x64)

## 1. Structure de Base

### Format des Instructions
- Une instruction par ligne
- Format : `instruction destination, source` ou `instruction opérande`
- Commentaires avec `;`

### Sections Programme
```assembly
section .data   ; Variables initialisées
section .bss    ; Variables non initialisées
section .text   ; Code exécutable
```

### Systèmes de Numération
| Type | Formats | Suffixes |
|------|---------|----------|
| Décimal | 5, 05, 0150d, 0d150 | d, t |
| Octal | 755q, 0q755 | q, o |
| Binaire | 0b11011101, 0b1101_1101 | b, y |
| Hexadécimal | 0xA5, 0A5h, 0hA5 | h, x |

## 2. Compilation et Exécution

### Linux x64
```bash
nasm -f elf64 program.asm -o program.o
ld program.o -o program
```

### Linux x32
```bash
nasm -f elf32 program.asm -o program.o
ld -m elf_i386 program.o -o program
```

### Windows
- Utilise des API calls via `extern`
- Sections spécifiques Windows
- Compilation différente (voir documentation Windows)

## 3. Directives Assembleur

### Architecture
```assembly
bits 64  ; Architecture 64 bits
bits 32  ; Architecture 32 bits
```

### Importation et Exportation
```assembly
extern printf     ; Importe fonction externe
global main       ; Exporte symbole
%include "file"   ; Inclut fichier externe
```

### Macros
```assembly
%define CONSTANT value    ; Définit constante
```

## 4. Variables et Types de Données

### Déclaration de Variables
```assembly
message db 'Hello, World!', 0    ; Chaîne
number dd 42                     ; Entier 32 bits
buffer resb 64                   ; Réserve 64 octets
```

### Réservation de Mémoire
| Directive | Taille | Description |
|-----------|--------|-------------|
| resb | 8 bits | Octets |
| resw | 16 bits | Mots |
| resd | 32 bits | Double-mots |
| resq | 64 bits | Quadruple-mots |

### Manipulation de Données
```assembly
length equ $-message            ; Calcule longueur
times 5 db 0                    ; Répétition
```

## 5. Instructions de Base

### Opérations Arithmétiques
| Instruction | Description | Exemple | Notes |
|-------------|-------------|---------|-------|
| ADD | Addition | `ADD eax, ebx` | Affecte CF, OF |
| SUB | Soustraction | `SUB ecx, 5` | Affecte CF |
| MUL | Multiplication non signée | `MUL ebx` | EDX:EAX |
| IMUL | Multiplication signée | `IMUL eax, 2` | |
| DIV | Division non signée | `DIV ecx` | EDX:EAX |
| IDIV | Division signée | `IDIV ebx` | |
| NEG | Inverse signe | `NEG eax` | |
| INC/DEC | Incrémente/Décrémente | `INC edx` | |

### Opérations Logiques
| Instruction | Description | Usage Typique |
|-------------|-------------|---------------|
| AND | ET logique | Masquage bits |
| OR | OU logique | Activation bits |
| XOR | OU exclusif | Mise à zéro |
| NOT | Inverse bits | Complément |
| SHL/SAL | Décalage gauche | Multiplication par 2^n |
| SHR | Décalage droite | Division par 2^n |
| ROL/ROR | Rotation | Rotation circulaire |

### Manipulation de Chaînes
| Instruction | Description |
|-------------|-------------|
| MOVSB/W/D | Copie |
| STOSB/W/D | Remplissage |
| LODSB/W/D | Chargement |
| SCASB/W/D | Comparaison |
| CMPSB/W/D | Comparaison mémoire |

## 6. Contrôle de Flux

### Sauts Conditionnels
| Instruction | Description | Condition |
|-------------|-------------|-----------|
| JMP | Inconditionnel | - |
| JE/JZ | Égal/Zéro | ZF = 1 |
| JNE/JNZ | Non égal | ZF = 0 |
| JG | Plus grand | ZF = 0, SF = OF |
| JL | Plus petit | SF ≠ OF |
| JGE | Plus grand/égal | SF = OF |
| JLE | Plus petit/égal | ZF = 1 ou SF ≠ OF |

### Boucles
```assembly
; Avec LOOP
MOV ecx, 10
debut:
    ; Instructions
    LOOP debut

; Avec test explicite
MOV ecx, 10
debut:
    ; Instructions
    DEC ecx
    JNZ debut
```

## 7. Registres et Flags

### Flags Importants
| Flag | Nom | Description |
|------|-----|-------------|
| CF | Carry | Report arithmétique |
| ZF | Zero | Résultat nul |
| SF | Sign | Résultat négatif |
| OF | Overflow | Débordement signé |
| PF | Parity | Parité |
| AF | Auxiliary | Report BCD |
| DF | Direction | Direction chaînes |

**Registres
https://github.com/jasonchampagne/FormationVideo/blob/master/Ressources/Assembleur/conventions-appel.md
## 8. Optimisations

### Techniques Courantes
```assembly
; Mise à zéro rapide
XOR eax, eax    ; Plus rapide que MOV eax, 0

; Multiplication par 2^n
SHL eax, n      ; Plus rapide que IMUL

; Test de bit
TEST al, 0x80   ; Test bit 7
```

### Bonnes Pratiques
1. Commenter le code important
2. Utiliser des étiquettes descriptives
3. Aligner et indenter correctement
4. Vérifier les flags après opérations
5. Sauvegarder/restaurer les registres modifiés

## 9. Différences x32/x64

### Principales Différences
- Taille des registres
- Conventions d'appel système
- Format de compilation
- En x32 : `int 0x80` au lieu de `syscall`
- En x32 : write = 4, en x64 = 1

## 10. La Pile (Stack)

### Concepts de Base

- Structure LIFO (Last In, First Out)
- Croît vers les adresses basses
- RSP (Stack Pointer) pointe vers le sommet
- RBP (Base Pointer) utilisé comme référence pour les variables locales

### Instructions de Base

|Instruction|Description|Exemple|Notes|
|---|---|---|---|
|PUSH|Empile une valeur|`PUSH rax`|Décrémente RSP puis stocke|
|POP|Dépile une valeur|`POP rbx`|Charge puis incrémente RSP|
|PUSHF|Empile les flags|`PUSHF`|Sauvegarde EFLAGS/RFLAGS|
|POPF|Dépile les flags|`POPF`|Restaure EFLAGS/RFLAGS|

### Gestion du Frame Pointer

; Exemple d'alignement
push rbp
mov rbp, rsp
and rsp, -16       ; Aligne RSP sur 16 octets

### Messages d'Erreur Courants
- "Segmentation fault" : Accès mémoire invalide
- "Invalid opcode" : Instruction non reconnue
- "Invalid operand" : Mauvais opérande
- "Symbol not found" : Étiquette manquante

