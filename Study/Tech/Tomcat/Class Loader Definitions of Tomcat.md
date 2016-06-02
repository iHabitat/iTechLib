## Class Loader Definitions of Tomcat

from the perspective of a web application, class or resource loading looks in the following repositories, in this order:

- Bootstrap classes of your JVM
- /WEB-INF/classes of your web application
- /WEB-INF/lib/*.jar of your web application
- System class loader classes (described above)
- Common class loader classes (described above)

If the web application class loader is configuered with delegate="true" then the order becomes:

- Bootstrap classes of your JVM
- System class loader classes (described above)
- Common class loader classes (described above)
- /WEB-INF/classes of your web application
- /WEB-INF/lib/*.jar of your web application