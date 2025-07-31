# PropertyAccessor

`PropertyAccessor` is a high-performance wrapper around `PropertyInfo` that enables fast property get/set operations using compiled delegates. This avoids the overhead of traditional reflection and provides fallback mechanisms when compilation is not possible.

---

## ✅ Features

- Fast property access using delegates
- Built-in default value caching for common value types
- Nullable enum support
- Reflection fallback with error tracking

---

## 🚀 Creating a PropertyAccessor

```csharp
PropertyInfo property = typeof(User).GetProperty("Name");
IPropertyAccessor accessor = PropertyAccessor.Create(property);
```

---

## 🛠️ Usage

```csharp
PropertyInfo property = typeof(User).GetProperty("Name");
var accessor = PropertyAccessor.Create(property);

var user = new User { Name = "Alice" };

// Get name, "Alice"
var name = accessor.GetValue(user);

// Set value to "Bob"
accessor.SetValue(user, "Bob");

// Clear value, setting it to default of property type. In this case default(string) = null
accessor.ClearValue(user);
```

---

## 💡 Default Value Handling

`PropertyAccessor` caches and provides default values for:

- Primitive types: `int`, `bool`, `float`, etc.
- Structs: `DateTime`, `TimeSpan`, `Guid`
- Nullable enums with special setter logic

---

## 🔍 Reflection Fallback

If delegate creation fails, `PropertyAccessor` will fall back to standard reflection:
In this mode, property values are accessed using reflection, which is slower but ensures the operation still completes. The fallback reason is recorded, allowing you to monitor when and why reflection fallback is used.


```csharp
if (accessor.UsesReflectionFallback)
{
    Console.WriteLine(accessor.Exception);
}
```

---

## ⚙️ Internal Optimization

- Uses static caches for default values and type combinations
- Compiles delegates at runtime for each property
- Fallback strategy to maintain compatibility

---

## 🧪 Example: Mapping with PropertyAccessor

```csharp
var source = new User { Name = "John" };
var target = new User();

var nameAccessor = PropertyAccessor.Create(typeof(User).GetProperty("Name"));
nameAccessor.SetValue(target, nameAccessor.GetValue(source));
```

---

## 🧼 License

MIT or use freely in your project.
