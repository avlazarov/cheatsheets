# Preload library

```
// preloaded.c
#define _GNU_SOURCE // Needed for RTLD_NEXT to be available
#include <dlfcn.h>
#include <stddef.h>
#include <stdio.h>

int (*original_random)(void);

int rand(){
  original_random = dlsym(RTLD_NEXT, "rand");

  if(original_random == NULL) {
    printf("No next rand() function");
    return -1;
  }

  printf("Original rand() was going to return %d\n", original_random());

  return 42;
}

// main.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>

int main(){
  srand(time(NULL));

  while(true){
    printf("Rand: %d\n", rand());
    sleep(1);
  }

  return 0;
}
```

```
$ gcc -fPIC -shared -o preloaded.so preloaded.c -ldl
$ gcc -o main main.c

$ ./main
$ LD_PRELOAD=preloaded.so ./main
```
