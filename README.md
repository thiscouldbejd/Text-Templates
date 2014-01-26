Text-Templates
==============

Description
-----------
Structured Text Templates ([T4](http://msdn.microsoft.com/en-us/library/bb126445.aspx "T4 Documentation")) for .Net Classes Code Generation.

Background
----------
These templates are a simple yet powerful and extendable framework for class-based code generation in .Net. The current implementation is in VB.NET (for all the code) and currently only generates VB.NET code (although it's extensibility allows for added C# too if the templates are written - I haven't yet had the need to do so).

They sprung into being due to my inability to find a sensible code generation toolkit, coupled with the need to write a lot of (relatively similar) class objects in VB.NET. I needed a mechanism to generate code that could be used immediately, without any extra dependencies, and the ability to define certain extra 'behaviours' for each class (e.g. simple DB retrieval, simple interface implementations). I wanted to use an established and well-supported templating language (T4) but also give myself enough freedom to generate exactly the code I wanted.

As a personal recommendation, I use them almost every day in some form or another. They are great for time-saving and consistency in projects that require extensive yet repetitive models/data-models. The framework have been relatively stable for a number of years which I hope shows a degree of usefulness and sensibleness in the design, rather than a lack of ambition on my part! I've added new behaviours when needed, and tweaked older ones with new ideas, but haven't needed to make any significant structural/breaking changes. I regard this is vital as I don't want to have to go through and change all my previous templates - I want them just to work when required.

Example
-------

An example todo.tt file might be as follows:

	<# Add_Class(a:="Todo", m:="Db-Table:tbl_ToDo") #>
	<# Add_Behaviour(n:="Comparable", p:="Fd_Comparable:Todo_Date") #>
	<# Add_Behaviour(n:="DbRetrievable") #>
	<# Add_Behaviour(n:="Formattable", p:="Fo_Names:|Fo_Values:{Todo_Title} - [{Todo_Date}]") #>
	<# Add_Field(n:="Id", t:="System.Int32", a:="R", m:="Db-Field:Id|Db-Key") #>
	<# Add_Field(n:="Todo_Date", t:="System.DateTime", m:="Db-Field:Date", f:="DateTime.Now") #>
	<# Add_Field(n:="Todo_Title", t:="System.String", a:="R", m:="Db-Field:Asset_Details") #>
	<# Add_Field(n:="Todo_Details", t:="System.Int32", m:="Db-Field:Attribute_Type") #>
	<#@ include file="%TEMPLATES_PATH%\Classes\VB_Object.tt" #>

This would generate a VB.NET class that would have 4 fields (e.g. variables/properties) and 4 different 'behaviours'. In this case, there would be a custom implementation of 'IComparable' (comparing the values of the Todo_Date field), a custom 'ToString/IFormattable' implementation and the ability to retrieve/save to a standard .Net Database (e.g. SQL or OLEDB). Minimal hassle, maximum functionality. The output code is a partial class, allowing you to add further code as required.

Usage
-----
You will need an environmental variable entitled **TEMPLATES_PATH** on your system which points to the root directory of these templates (in order to allow them to be loaded and applied dynamically). Without this, they will not work and you'll get lots of unhelpful transformation errors when you try to use them.

You can then use a tool, such as Microsoft's [TextTransform](http://msdn.microsoft.com/en-us/library/bb126245.aspx "TextTransform Documentation") command line transformation tool, to generate code files from the templates. Simply transform your class .tt file into an output code file.

Limitations
-----------
- There is a significant amount of further documentation to be written, to help explain some of the design decisions made. I've tried to make them as readable, and as consistent in naming, as possible - but you can never have too much explanation.
- A signification amount of the templates work goes into code formatting, this is just the way I like things, but it may not suit everyone!
- Some of the 'behaviours' committed generate code that refers to other closed-source libraries and tools (Attributable, Collection, Command, CommandFlag, Configurable, Convertible, Described, Editable, FixedWidthWriteable, Hashable, Locateable, NetworkInterface, Serialisable, SnmpManageable, Snmp-Mapping, ThreadSafeUpdates, Unique, Singleton). It is my intention to release the tools that are referred to in the near future, so I decided to commit these templates as well (for the sake of completeness).
