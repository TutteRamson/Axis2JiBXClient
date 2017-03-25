# Axis2JiBXClient
You want to generate a Java client for a WebService from its WSDL file. This documentation gives you the step by step instructions.
 
#### WORK IN PROGRESS

## Types of Axis2 Clients
With Axis2 you can generate clients using 3 types of bindings.
1. [Axis Data Binding aka ADB](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients.html#adb)
2. [XML Beans](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients-xmlbeans.html)
3. [JiBX Java XML Binding](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients-jibx.html)

As stated in the [Axis2 Documentation](https://axis.apache.org/axis2/java/core/docs/userguide-creatingclients.html#createclients) ADB is the most
straightforward way to generate Java clients but it has limitations when it comes to dealing with XML elements that extend other elements,
i.e., polymorphic elements. If the response of your webservice call contains polymorphic elements that are nested,
`ADBException: Unexpected subelement` hits you hard while parsing the response into Java Objects.

>> XMLBeans: Unlike ADB, XMLBeans is a fully functional schema compiler, so it doesn't carry the same limitations as ADB. It is, however, a bit more complicated to use than ADB. It generates a huge number of files, and the programming model, while being certainly usable, is not as straightforward as ADB.

>> JiBX: JiBX is a complete databinding framework that actually provides not only WSDL-to-Java conversion, as covered in this document, but also Java-to-XML conversion. In some ways, JiBX provides the best of both worlds. JiBX is extremely flexible, enabling you to choose the classes that represent your entities, but it can be complicated to set up. On the other hand, once it is set up, actually using the generated code is as easy as using ADB.

## Generate Java Client using Axis2 and JiBX
The instructions provided here are based on the most recent versions of [Axis2 1.7.4](https://axis.apache.org/axis2/java/core/release-notes/1.7.4.html) and
[JiBX 1.3.1](https://sourceforge.net/projects/jibx/files/) that are available as of this writing.

### Step 1: JiBX CodeGen

### Step 2: Axis2 WSDL2Java

### Step 3: JiBX Binding

### Step 4: Create Client with needed Dependencies




## Reference
* [Axis2 WSDL2Java Reference](https://axis.apache.org/axis2/java/core/docs/reference.html)
* [Axis2 documentation on generating client using JiBX binding](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients-jibx.html)
* [JiBX CodeGen documentation](http://jibx.sourceforge.net/fromschema/codegen.html)
* [JiBX Binding documentation](http://jibx.sourceforge.net/bindcomp.html)