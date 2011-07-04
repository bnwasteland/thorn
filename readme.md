Thorn (Thor for .NET)
=====================
A command line utility accelerator.

In 30 secs
----------
For building a "Tasks" style command line utility. To wit:
	
1. In VS2010, Create a new "Console Application" project.
2. Reference Thorn. (NuGet coming soon)
3. Make your code look like this:

Program.cs:

	class Program
	{
		static void Main(string[] args)
		{
			Thorn.Runner.Run(args);
		}
	}

Exports.cs:

	[ThornExport]
	public class Exports
	{
		[Description("Says 'Hello'")]
		public void Hello(HelloOptions opts)
		{
			Console.WriteLine("Hello, {0}!", opts.Target);
		}
	}

	public class HelloOptions
	{
		[Description("Object of the salutation")]
		public string Target { get; set; }
	}

##### Run it at the commandline:
	
	C:\...\bin\Debug>MyApp hello /T World

##### Get Usage Info:
	
	C:\...\bin\Debug>MyApp help
	C:\...\bin\Debug>MyApp help hello

In 2 Minutes
------------
To facilitate zero-friction "exports to commandline", Thorn makes a few assumptions.

First it discovers types and methods to be exported via a reflection scan. The default
convention scans the entry assembly for types marked with the `[ThornExport]` attribute
and exports the public methods of those types. Methods marked with the `[ThornIgnore]`
attribute will be skipped.

When an exported method is invoked, it's parameter type is examined and bound from the 
commandline switches by cbrianball's excellent Args library. Thorn also leans on Args to
generate the help text about available switches to a command.

Therefore there are a couple of simple requirements on exported types and methods:

- Exported types must be public and instantiable (non-abstract, non-static)
- In the default configuration, exported types must have a default constructor
- Exported methods must be instance methods
- Exported methods must be parameterless or have exactly 1 parameter
- Parameter may be complex. See Args docs for details

#### Overriding the defaults

You have the opportunity to bend and break a number of these rules. To set advanced 
configuration options, use an initializer as below:

	var runner = Thorn.Runner.Configure(config => {
		config.DoNotScan();
		config.Export(typeof(MyUndecoratedType));

		config.UseTypeScanningConvention(new MyTypeScanningConvention());
	});

	runner.Run(args);

MIT License
-----------
Copyright (c) 2011 Jace Bennett and contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.