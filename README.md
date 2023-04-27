# FOP.NetCore
**FOP.NetCore** is an open source port of the latest Apache FOP (`Version 2.8`) for .NET Core. Now you can generate PDFs in .NET Core supported by the latest version Apache FOP.

This project will work on `.NET Core 3.1` and beyond (`.NET Core 5`, `.NET Core 6`, `.NET Core 7`, etc.)

## Nuget Package
[![NuGet version (FOP.NetCore)](https://img.shields.io/nuget/v/FOP.NetCore.svg?style=flat-square)](https://www.nuget.org/packages/FOP.NetCore/)

## A Little History
[Apache FOP](https://xmlgraphics.apache.org/fop/) is an open-source print formatter that formats XSL-FO documents into various output formats, including PDF, PostScript, and others. Initially, Apache FOP was designed to work with Java-based systems, but it can also be used with .NET-based systems using different ports.

### **FO.NET**

One of the earliest ports of Apache FOP to .NET Framework was FO.NET. This was an early attempt to provide a .NET implementation of Apache FOP, and it was based on an early pre-v1.0 version of Apache FOP. FO.NET was fully managed code, which made it suitable for use in .NET Framework environments. Despite being old and unsupported, FO.NET is still functional and can be used in .NET Framework environments. However, it lacks many of the features and updates that have been added to Apache FOP over the years. Additionally, it is only supported .NET Framework environments and will not work in .NET Core projects.

### **Crispin.FOP**

Another port was Crispin.FOP - this is a .NET Framework library that provides a wrapper around Apache FOP Version 2.3. The library allows .NET developers to easily use Apache FOP within their applications without having to write Java code. However, it is only supported in .NET Framework environments and will not work in .NET Core projects.

### **ApacheFOP.Serverless**

More recently, Brandon Bernard released ApacheFOP.Serverless. This is a lightweight, serverless implementation of Apache FOP that can be used in various environments. It is designed to be deployed as a serverless Azure function on various cloud providers, and it can be used to generate PDF documents from XSL-FO input files. It relies on the fact that Azure functions support running Java applications. However, it does not allow us to generate our PDF files directly in our application - you have to connect to your Azure functions to transform and generate your PDF files. Under the hood, it is using the Java Apache FOP files - so its clean, but has to be HOSTED.

🤔 Wouldn't it be great if we could just write some code and generate a PDF file? 

😭 Why does it have to be so hard?


## The Solution
**FOP.NetCore** is a new library that offers a pure port of the latest Apache FOP project to .NET Core. With FOP.NetCore, you can now generate your XSL-FO based PDF files directly in your .NET Core project. By simply adding the Nuget package to your .NET Core project, you can now generate PDF files in .NET Core using this library. 

This project is based on the great work provided by the IKVM-Revived project. [IKVM-Revived](https://en.wikipedia.org/wiki/IKVM.NET) is a free and open-source implementation of the Java Virtual Machine (JVM) for the .NET Framework. It allows Java code to be run on the .NET platform, providing interoperability between Java and .NET environments. The latest version also allows java code to be run in .NET Core. Just think of it like this: once you install the IKVM Nuget package to your project, you can write java code using .NET.

## The Result
A pure port of Apache FOP that can be used in .NET Core projects. 👍👍

<a href="https://www.buymeacoffee.com/sorcerdon" target="_blank">
<img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174">
</a> 

I'm thirsty...

# Installation & Usage
1. **Install Nuget Package** 

    Install the FOP.NetCore nuget package and its dependency (IKVM 8.4.5)

    [![NuGet version (FOP.NetCore)](https://img.shields.io/nuget/v/FOP.NetCore.svg?style=flat-square)](https://www.nuget.org/packages/FOP.NetCore/)

2. **FOP Config File** 

    You will need a `fop.xconf` file included in this repo. This file just defines the settings for FOP. Or You can get this file by downloading it directly from the Apache FOP project.

3. **Sample Code**

    Generate the XSL-FO file (XML to FO) and then transform that into a PDF using the following sample code:

    ```
    using System;
    using System.IO;
    using System.Xml;
    using System.Xml.Xsl;
    using java.io;
    
    public class Program
    {
        static void Main(string[] args)
        {
            string foConfFilePath = @"<PATH TO>\fop.xconf";
            string xmlFilePath = @"<PATH TO>\myxml.xml";
            string xsltFilePath = @"<PATH TO>\myxslt.xslt";
            string outputPath = @"<PATH TO>\final.pdf";

            //READ THE XML IN AS A STRING
            string xml = System.IO.File.ReadAllText(xmlFilePath);

            //GENERATE FO
            byte[] xslFo = GenerateXSLFO(xml, xsltFilePath);

            //GENERATE PDF
            byte[] pdfByteArray = GeneratePDF(xslFo, foConfFilePath);

            //SAVE PDF
            System.IO.File.WriteAllBytes(outputPath, pdfByteArray);
        }

        private static byte[] GenerateXSLFO(string xml, string xsltFilePath)
        {
            using (XmlReader xmlReader = XmlReader.Create(new System.IO.StringReader(xml)))
            using (var ms = new MemoryStream())
            {
                var xslt = new XslCompiledTransform(true);
                xslt.Load(xsltFilePath);
                xslt.Transform(xmlReader, null, ms);
                ms.Position = 0;
                return ms.ToArray();
            }
        }

        public static byte[] GeneratePDF(byte[] inputFo, string fopConfigFilePath)
        {
            byte[] finalPDF;

            ByteArrayOutputStream os = new ByteArrayOutputStream();
            try
            {
                var fopFactory = (org.apache.fop.apps.FopFactory)org.apache.fop.apps.FopFactory.newInstance(new java.io.File(fopConfigFilePath));
                var fop = fopFactory.newFop("application/pdf", os);
                javax.xml.transform.TransformerFactory factory = javax.xml.transform.TransformerFactory.newInstance();
                javax.xml.transform.Transformer transformer = factory.newTransformer();

                InputStream input = new ByteArrayInputStream(inputFo);
                javax.xml.transform.Source src = new javax.xml.transform.stream.StreamSource(input);
                javax.xml.transform.Result res = new javax.xml.transform.sax.SAXResult(fop.getDefaultHandler());
                transformer.transform(src, res);

                finalPDF = os.toByteArray();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                os.close();
            }

            return finalPDF;
        }
    }
    ```

    You can modify this code as necessary to match your requirements.
