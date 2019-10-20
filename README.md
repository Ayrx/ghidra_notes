# Reference resources files from code

To package resource files in an extension, add it to the `data/` directory
and use `Application.getModuleDataFile` to access it. The method returns a
`ResourceFile` object and `getFile` can be called if you require a `File`
object.

```java
ResourceFile dataFile = Application.getModuleDataFile("JNIAnalyzer", "jni_all.gdt");
File f = dataFile.getFile(true)
```

Reference: https://github.com/NationalSecurityAgency/ghidra/issues/976
