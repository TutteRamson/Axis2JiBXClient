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
JiBX CodeGen tool generates Java Objects for the XML elements that are present in the WSDL or in the XSD files referenced by the WSDL. This tool also generates a binding file
that is needed for Step 2.

1. Download the latest version of [JiBX 1.3.1](https://sourceforge.net/projects/jibx/files/) and Unzip the contents. The lib directory has `jibx-tools.jar` that has CodeGen tool.
2. The WSDL file has elements referenced either inline or by means of XSD files in the schema section.
    ```
      <wsdl:types>
        <schema>
        ...
        ...
        </schema>
      </wsdl:types>
    ```
   If the schema is inline extract that as an XSD file as explained [here](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients-jibx.html). If the schema isn't defined inline
   and it refereces externally hosted XSD files, we just need the URLs of the top level XSD file(s). CodeGen will automatically load other XSD schemas refrenced by the top level XSD file and include them in the code generation.
3. Navigate to the `lib` directory of JiBX and run the following [CodeGen](http://jibx.sourceforge.net/fromschema/codegen.html) command to generate java classes and the binding file:

    `java -Xms512M -Xmx512M -cp ./jibx-tools.jar org.jibx.schema.codegen.CodeGen -p com.yourCompany.axis2.jibx.packageName -t /Users/directory/path/for/yourService -w "http://remote.webservice.com/yourService.svc?xsd=xsd0"`
4. This generates *binding.xml* file and java classes in the directory */Users/directory/path/for/yourService/com/yourCompany/axis2/jibx/packageName*.

### Step 2: Axis2 WSDL2Java
The steps below will generate the stub needed to create the java client for the web service. Axis2 needs the java classes generated from the previous step to create the stub.
1. Download the binary distribution of the latest version of [Axis2 1.7.4](http://axis.apache.org/axis2/java/core/download.html) and Unzip the contents.
2. Navigate to the `bin` directory and run the [`wsdl2java`](https://axis.apache.org/axis2/java/core/docs/reference.html) command.

    `./wsdl2java.sh -o /Users/directory/path/for/yourService -s -p com.yourCompany.axis2.jibx.packageName -d jibx -Ebindingfile /Users/directory/path/for/yourService/binding.xml -sp -uri "http://remote.webservice.com/yourService.svc?wsdl"`
3. This generates `build.xml` file and `/src` directory with Axis2 generated java classes in */Users/directory/path/for/yourService*.
4. Move the JiBX generated java classes that are under */Users/directory/path/for/yourService/com/yourCompany/axis2/jibx/packageName* into the same `/src` directory so that both Axis2 and JiBX generated classes are in */Users/directory/path/for/yourService/src/com/yourCompany/axis2/jibx/packageName*.
5. Kick start the Ant build using `build.xml`. If you are using Eclipse, set up Environment Variable *AXIS2_HOME* with the directory path where you have Unziped Axis2 1.7.4 as the value. If you are using IntelliJ, in the Ant build set up a property *env.AXIS2_HOME* with the same directory path as the value.
6. This will generate a `jar` file with the name specified in the `build.xml`. Feel free to change the name of the jar by updating `build.xml`.

### Step 3: JiBX Binding
The above step compiles most of the available classes, but not everything. Fortunately, it does compile the classes needed by the JiBX compiler, so you can now generate the actual JiBX resources.
With the JiBX binding compiler we need to compile the bindings into the class files generated.
1. Navigate to the `build/classes` directory. It our example it is `/Users/directory/path/for/yourService/build/classes`.
2. Run the following command to compile the bindings:

    `java -jar /Users/directory/path/jibx/lib/jibx-bind.jar -v /Users/directory/path/for/yourService/binding.xml`
3. This will generate several class prefixed with `JiBX_binding...` in `/Users/directory/path/for/yourService/build/classes`.
4. Re-run the Ant task again as explained in Step 2 above. The generated jar file will have the bindings included.


### Step 4: Create Client with needed Dependencies




## Reference
* [Axis2 WSDL2Java Reference](https://axis.apache.org/axis2/java/core/docs/reference.html)
* [Axis2 documentation on generating client using JiBX binding](http://axis.apache.org/axis2/java/core/docs/userguide-creatingclients-jibx.html)
* [JiBX CodeGen documentation](http://jibx.sourceforge.net/fromschema/codegen.html)
* [JiBX Binding documentation](http://jibx.sourceforge.net/bindcomp.html)