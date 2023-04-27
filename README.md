# FOP.NetCore
Latest Apache FOP (Version 2.8) for .NET Core so that you can use FOP in your .NET Core projects.
Now you can generate PDFs in .NET Core supported by the latest version Apache FOP.

## Nuget Package
[![NuGet version (FOP.NetCore)](https://img.shields.io/nuget/v/FOP.NetCore.svg?style=flat-square)](https://www.nuget.org/packages/FOP.NetCore/)

## A Little History
Apache FOP is an open-source print formatter that formats XSL-FO documents into various output formats, including PDF, PostScript, and others. Initially, Apache FOP was designed to work with Java-based systems, but it can also be used with .NET-based systems using different ports.

One of the earliest ports of Apache FOP to .NET Framework was FO.NET. This was an early attempt to provide a .NET implementation of Apache FOP, and it was based on an early pre-v1.0 version of Apache FOP. FO.NET was fully managed code, which made it suitable for use in .NET Framework environments. Despite being old and unsupported, FO.NET is still functional and can be used in .NET Framework environments. However, it lacks many of the features and updates that have been added to Apache FOP over the years. Additionally, it is only supported .NET Framework environments and will not work in .NET Core projects.

Another port was Crispin.FOP - this is a .NET Framework library that provides a wrapper around Apache FOP Version 2.3. The library allows .NET developers to easily use Apache FOP within their applications without having to write Java code. However, it is only supported in .NET Framework environments and will not work in .NET Core projects.

More recently, Brandon Bernard released ApacheFOP.Serverless. This is a lightweight, serverless implementation of Apache FOP that can be used in various environments, including .NET Core. It is designed to be deployed as a serverless Azure function on various cloud providers, and it can be used to generate PDF documents from XSL-FO input files. It relies on the fact that Azure functions support running Java applications. However, it does not allow us to generate our PDF files in memory - you have to connect to your Azure functions to transform and generate your PDF files. Under the hood, it is using a pure version of Apache FOP - so its clean, but has to be HOSTED.

Wouldn't it be great if we could just call a method directly in our .NET Core application and generate a PDF file?


## The Solution
FOP.NetCore is a new library that offers a pure port of the latest Apache FOP project to .NET Core. With FOP.NetCore, you can now generate your XSL-FO based PDF files directly in .NET Core. This library is a pure port, which means that it has all the features of Apache FOP. By simply adding the Nuget package to your .NET Core project, you can now generate PDF files in .NET Core using this library.

This project is based on the great work provided by the IKVM-Revived project. IKVM-Revived is a free and open-source implementation of the Java Virtual Machine (JVM) for the .NET Framework. It allows Java code to be run on the .NET platform, providing interoperability between Java and .NET environments. The latest version also allows java code to be run in .NET Core. Just think of it like this: once you install the IKVM Nuget package to your project, you can write java code using .NET.

## The Result
A pure .NET Core port of Apache FOP using IKVM.


# Installation
1. Install the FOP.NetCore nuget package and its dependency (IKVM 8.4.5)
2. You will need a `fop.xconf` file. This file just defines the settings for FOP. You can get this file by downloading it directly from the Apache FOP project. I also have included it in this repo.


