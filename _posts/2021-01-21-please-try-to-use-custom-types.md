---
layout: post
title: "Please, try to use custom types"
categories: design
author:
- Daniel
published: true
---

Consider these two different implementations. I have only made a custom type of the CVR, but should also create types for ContactEmail and SE.

```csharp
    class LooseCompany
    {
        public string Name { get; set; }
        public string ContactEmail { get; set; }
        public int CVR { get; set; }
        public int SE { get; set; }
    }
```

```csharp
    class TighterCompany
    {
        public string Name { get; set; }
        public string ContactEmail { get; set; }
        public CVR CVR { get; set; }
        public int SE { get; set; }
    }
```

Sure, we can agree that a CVR can be an int. But I feel robbed from a few things really, which I get when using a custom type instead.

- Validity of a custom type can and should be built in
- No guessing of what I am working with
- Enriches your code due to clearer types

Would you rather work with a int

![image](/assets/images/int.png)

Or would you rather work with a custom type ?

![image](/assets/images/cvr.png)

With the "int" implementation you can keep guessing all day to what the int actually is. With the CVR type, you are not in doubt.
