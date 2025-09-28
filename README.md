<div align="center">

# 🚀 Pyrex


## 🛠️ Installation

```bash
pip install pyrex3
```

**Requirements:**  
You’ll need `rustc`, `gcc`/`clang`, and `g++`/`clang++` installed.

---

## 🚀 Quick Start

### Rust Example
```python
from pyrex.languages.rust import rust

result = rust.execute("""
    let numbers = vec![1, 2, 3, 4, 5];
    let sum: i32 = numbers.iter().sum();
    println!("Sum: {}", sum);
""")
print(result)  # "Sum: 15"
```

### C Example
```python
from pyrex import c

result = c.execute("""
    int result = x * y + z;
    printf("Result: %d\\n", result);
""", {"x": 10, "y": 20, "z": 5}, fast=True)

print(result)  # "Result: 205"
```

### C++ Example
```python
from pyrex import cpp

result = cpp.execute("""
    std::vector<int> data = numbers;
    std::sort(data.begin(), data.end());

    std::cout << "Sorted: ";
    for (int n : data) std::cout << n << " ";
    std::cout << std::endl;
""", variables={"numbers": [64, 34, 25, 12, 22, 11, 90]})

print(result)
```

---

## 📚 Advanced Usage

### Custom Compiler Settings
```python
from pyrex.languages.rust import RustCompiler
from pyrex.core.base import CompilerConfig


compiler = RustCompiler(
    config=CompilerConfig(
        compile_flags=["-O", "--edition", "2021"],
        cache_dir="/tmp/pyrex_cache",
        enable_security=True
    )
)


result = compiler.execute("""
    let result = (0..1_000_000).sum::<i64>();
    println!("Sum: {}", result);
""")

print(result)

```

### Error Handling
```python
from pyrex.exceptions import PyrexCompileError

try:
    rust.execute("let x = ;")  # Invalid syntax
except PyrexCompileError as e:
    print(f"Error:\n    {e}")
```

---

## 🎯 Type Mapping

| Python Type  | Rust        | C           | C++                  |
|--------------|-------------|-------------|----------------------|
| `bool`       | `bool`      | `bool`      | `bool`              |
| `int`        | `i64`      | `long long` | `long long`          |
| `float`      | `f64`      | `double`    | `double`             |
| `str`        | `String`    | `char*`     | `std::string`        |
| `list[int]`  | `Vec<i64>`  | `int[]`     | `std::vector<int>`   |

---

## Performance

- **First run:** Compiles & caches the binary  
- **Next runs:** Executes instantly (10–100× faster)  
- **Smart invalidation:** Cache refreshes automatically when code or variables change  

---

## 📖 API Reference

### `execute(code, variables={"key": "value"}, timeout=30.0, force_recompile=False, fast=True)`

**Parameters:**  
- `code` *(str)* – Source code to compile and run  
- `variables` *(dict, optional)* – Injected variables  
- `timeout` *(float, default=30s)* – Max runtime  
- `force_recompile` *(bool)* – Ignore cache, force rebuild 
- `fast` *(bool)*  – Skips few checks to get faster compilation

**Returns:**  
- Execution output as `str`

**Raises:**  
- `PyrexCompileError` – Compilation failed  
- `PyrexRuntimeError` – Runtime error  
- `PyrexTypeError` – Type conversion issue  
- `PyrexSecurityError` – Security violation  

---

## 📄 License

Licensed under the MIT License. See the [LICENSE](LICENSE) file.

---

## 🙏 Acknowledgments

- Built with ❤️ by Luciano Correia  
