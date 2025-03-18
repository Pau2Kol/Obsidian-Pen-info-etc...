
`#include<unistd.h>
`void main()
`{ setuid(0);
  `setgid(0);
`  system("thm");
`}`

gcc "nom" -o path -w
chmod u+s path
