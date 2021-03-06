# SharpEval

SharpEval is a library that allows you to parse & evaluate math expressions at runtime

## Features

- Parses and evaluates expressions extremely fast
- Supports custom variables and functions
- Compatible with Unity 3.4.0 and newer
- Supports Unity IL2CPP
- No reflection
- No external dependencies

## Usage

### Parse & evaluate a simple expression:

```cs
using SharpEval;
using SharpEval.Tokens;

internal static class Program
{
  internal static void Main()
  {
    IToken[] tokens = Tokenizer.Tokenize(" 2 * ( 2 + 2 ) / 2 ");
  
    Interpreter interpreter = new Interpreter();
  
    Console.WriteLine(interpreter.EvaluateExpression(tokens)); // 4
  }
}
```

### Parse & evaluate a more complex expression:

```cs
using SharpEval;
using SharpEval.Tokens;

internal static class Program
{
  internal static void Main()
  {
    IToken[] tokens = Tokenizer.Tokenize(" ( ( ( 2 + 2 ) ^ 2 + 16 ) - 2 ^ ( 4 + 4 ) ) / 2");
  
    Interpreter interpreter = new Interpreter();
  
    Console.WriteLine(interpreter.EvaluateExpression(tokens)); // -112
  }
}
```

### Define custom read-only variables:

```cs
using SharpEval;
using SharpEval.Tokens;
using SharpEval.Variables;

internal static class Program
{
  internal static void Main()
  {
    IToken[] tokens = Tokenizer.Tokenize(" x + y ");
    
    Dictionary<string, IVariable> variables = new Dictionary<string, IVariable>()
    {
        ["x"] = new ReadOnlyVariable(10.0d),
        ["y"] = new ReadOnlyVariable(20.0d),
    };
    
    Interpreter interpreter = new Interpreter(variables: variables);
    
    Console.WriteLine(interpreter.EvaluateExpression(tokens)); // 25
  }
}
```

### Define custom computed variables:

```cs
using SharpEval;
using SharpEval.Tokens;
using SharpEval.Variables;
using System;

internal static class Program
{
  internal static void Main()
  {
    IToken[] tokens = Tokenizer.Tokenize(" randomNumber + 1 ");
    
    Random random = new Random();
    
    Dictionary<string, IVariable> variables = new Dictionary<string, IVariable>()
    {
        ["randomNumber"] = new ComputedVariable(() => random.NextDouble()),
    };
  
    Interpreter interpreter = new Interpreter(variables: variables);
    
    Console.WriteLine(interpreter.EvaluateExpression(tokens));
  }
}
```

### Define custom functions:

```cs
using SharpEval;
using SharpEval.Tokens;
using System.Linq;

internal static class Program
{
  internal static void Main()
  {
    IToken[] tokens = Tokenizer.Tokenize(" sum( 1, 2, 3 ) ");
    
    Dictionary<string, Func<double[], double>> functions = new Dictionary<string, Func<double[], double>>()
    {
        ["sum"] = args => args.Sum(),
    };
    
    Interpreter interpreter = new Interpreter(functions: functions);
    
    Console.WriteLine(interpreter.EvaluateExpression(tokens)); // 6
  }
}
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Donation

If you like SharpEval consider [buying me a coffee](https://ko-fi.com/winterboltgames).
