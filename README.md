# WASM

## Guide
Using WASI: https://tinygo.org/docs/guides/webassembly/wasi/

## VScode requirements
If you want to compile code using syscall/js package,  
you need to add the following configuration to the `settings.json` file:
```json
"go.toolsEnvVars": {
        "GOOS": "js",
        "GOARCH": "wasm"
}

```


## Example
```go
package main

import (
    "syscall/js"
)

func main() {
    js.Global().Get("document").Call("write", "Hello, World!")
}
```


## Install
```bash
brew tap tinygo-org/tools
brew install tinygo
```

### Compile / Build
```bash
# Or with go with syscall/js
GOOS=js GOARCH=wasm go build -o main.wasm main.go


# Tinygo
tinygo build -o main.wasm -target=wasi main.go
# Or with Go
GOOS=wasip1 GOARCH=wasm go build -o main.wasm main.go
```

### Create html file
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Wasm Example</title>
</head>
<body>
    <script src="wasm_exec.js"></script>
    <script>
        const go = new Go();
        WebAssembly.instantiateStreaming(fetch("main.wasm"), go.importObject).then((result) => {
            go.run(result.instance);
        });
    </script>
</body>
</html>

```

### Copy wasm_exec.js to the project
```bash
cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .
```

# Runtime using wasmtime in terminal

## Install
```bash
brew install wasmtime
```

## Run
```bash
wasmtime main.wasm
```

