# Ghidra API Notes

## Reference resource files from code

To package resource files in an extension, add it to the `data/` directory
and use `Application.getModuleDataFile` to access it. The method returns a
`ResourceFile` object and `getFile` can be called if you require a `File`
object.

```java
ResourceFile dataFile = Application.getModuleDataFile("JNIAnalyzer", "jni_all.gdt");
File f = dataFile.getFile(true)
```

Reference: https://github.com/NationalSecurityAgency/ghidra/issues/976

## Iterate over all functions in a program

```java
Function function = this.getFirstFunction();
while (function != null) {
    // do stuff
    function = this.getFunctionAfter(function);
}
```

## Get references to and from a function

```java
Function f = ...

// Get references to a function
Set<Function> callers = f.getCallingFunctions(this.monitor)

// Get references from a function
Set<Function> callees = f.getCalledFunctions(this.monitor)
```

## Modifying function signature

```java
// Size of array depends on the number of parameters
Parameter[] params = new Parameter[2];

// Modify the `name` and `datatype` parameters accordingly
params[0] = new ParameterImpl(name, datatype, this.currentProgram, SourceType.USER_DEFINED);
params[1] = new ParameterImpl(name, datatype, this.currentProgram, SourceType.USER_DEFINED);

// Modify the `datatype` parameter accordingly
returnType = new ReturnParameterImpl(datatype, this.currentProgram);

// `returnType` and `params` were previously defined.
// Set the other parameters accordingly. Use `null` if no change is required.
function.updateFunction(
    callingConvention,
    returnType,
	Function.FunctionUpdateType.DYNAMIC_STORAGE_FORMAL_PARAMS,
	true,
	SourceType.USER_DEFINED,
	params
);
