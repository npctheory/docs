## Добавить в PATH:  
**csc.exe**: `C:\Windows\Microsoft.NET\Framework64\v4.0.30319`  
(Из этой директории компилятор достает MSCorLib.dll)  
**ildasm, gacutil**: `C:\Program Files (x86)\Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.8 Tools`  

## Скомпилировать один Program.cs в .exe
(Building Types into a Module)  
<details>
    <summary><code>csc.exe Program.cs</code> (Выполнить в папке, где есть Program.cs)</summary>
    Нужно использовать в файле <code>using System;</code>  
    вместо использования на компиляции <code>/r:MSCorLib.dll</code>  
    <code>/out:Program.exe /t:exe</code> задавать вручную не нужно, это значения по умолчанию  
</details> или задать параметры явно:
`csc.exe /out:Program.exe /t:exe /r:MSCorLib.dll Program.cs`

## Ildasm
`ILDasm Program.exe` в папке с Program.exe  
`Ctrl+M` посмотреть метадату  
`View/Statistics` посмотреть статистику  
<details>
<summary>Что такое CLR-assembly:</summary>
To summarize, an assembly is a unit of reuse, versioning, and security. It allows
you to partition your types and resources into separate files so that you, and consumers of
your assembly, get to determine which files to package together and deploy. After the CLR
loads the file containing the manifest, it can determine which of the assembly’s other files
contain the types and resources the application is referencing. Anyone consuming the assembly is required to know only the name of the file containing the manifest; the file partitioning is then abstracted away from the consumer and can change in the future without
breaking the application’s behavior.
If you have multiple types that can share a single version number and security settings, it
is recommended that you place all of the types in a single file rather than spread the types
out over separate files, let alone separate assemblies. The reason is performance. Loading
a file/assembly takes the CLR and Windows time to find the assembly, load it, and initialize
it. The fewer files/assemblies loaded the better, because loading fewer assemblies helps
reduce working set and also reduces fragmentation of a process’s address space. Finally,
NGen.exe can perform better optimizations when processing larger files.
</details>

## Скомпилировать один Program.cs в .dll
В папке с FUT.cs и RUT.cs
`csc /out:MultiFileLibrary.dll /t:library /addmodule:RUT.netmodule FUT.cs`

## Strongly named & weakly named assemblies

## Public Assembly Key