vb6grammar
==================================================

Visual Basic 6.0 Grammar for ANTLR4

This is an approximate grammar for Visual Basic 6.0, derived 
from the Visual Basic 6.0 language reference 
http://msdn.microsoft.com/en-us/library/aa338033%28v=vs.60%29.aspx 
and tested against MSDN VB6 statement examples as well as several Visual 
Basic 6.0 code repositories.


Characteristics:

1. This grammar is line-based and takes into account whitespace, so that
   member calls (e.g. "A.B") are distinguished from contextual object calls 
   in WITH statements (e.g. "A .B").

2. Keywords can be used as identifiers depending on the context, enabling
   e.g. "A.Type", but not "Type.B".


Execution:

```java
final java.io.File inputFile = new java.io.File("src/test/resources/org/vb6/gpl/statements/Print.cls");
final java.io.InputStream inputStream = new java.io.FileInputStream(inputFile);

/*
* lexer
*/
final org.antlr.v4.runtime.ANTLRInputStream antlrInputStream = new org.antlr.v4.runtime.ANTLRInputStream(inputStream);
final org.vb6.VisualBasic6Lexer lexer = new org.vb6.VisualBasic6Lexer(antlrInputStream);
final org.antlr.v4.runtime.CommonTokenStream tokens = new org.antlr.v4.runtime.CommonTokenStream(lexer);

/*
* parser
*/
final org.vb6.VisualBasic6Parser parser = new org.vb6.VisualBasic6Parser(tokens);
final org.vb6.VisualBasic6Parser.StartRuleContext ctx = parser.startRule();
```

```java
/*
 * traverse the abstract syntax tree (AST) with an ANTLR visitor
 */
final org.vb6.VisualBasic6BaseVisitor<Boolean> visitor = new org.vb6.VisualBasic6BaseVisitor<Boolean>() {
	/*
	 * exemplary callback function for print statement
	 */
	@Override
	public Boolean visitPrintStmt(final VisualBasic6Parser.PrintStmtContext ctx) {
		return visitChildren(ctx);
	}
};

visitor.visit(ctx);
```


VM Args:

* For parsing large VB6 source code files, following VM args have to be set: -Xmx2048m -XX:MaxPermSize=256m


Known limitations:

1. Preprocessor statements (#if, #else, ...) must not interfere with regular
   statements.

2. Comments are skipped.


Release process:

* Milestones are published in the [ANTLR grammars repo](https://github.com/antlr/grammars-v4).