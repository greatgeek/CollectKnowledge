#### Apache Tomcat

The Apache Tomcat software is an open source implementation of the Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies. The Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket specifications are developed under the [Java Community Process](http://jcp.org/en/introduction/overview).

The Apache Tomcat software is developed in an open and participatory environment and released under the [Apache License version 2](http://www.apache.org/licenses/). The Apache Tomcat project is intended to be a collaboration of the best-of-breed developers from around the world. We invite you to participate in this open development project. 

Apache Tomcat software powers numerous large-scale, mission-critical web applications across a diverse range of industries and organizations.

#### How does Apache Tomcat work?

Tomcat is widely used by web developers when working on web application development. From a high-level perspective,  apache tomcat is responsible to provide a run-time environment for the servlets. It provides an environment in which one could run their java code.

**On a more detailed aspect, tomcat is responsible for:**

1. Listen to all incoming requests from clients.
2. Load the respective servlet classes using the servlet mappings(from web.xml file) to handle incoming client requests.
3. Execute the servlet class.
4. Finally, unload the servlet class.

From the point the servlet class is loaded to the point it's unloaded, the servlet is responsible for handling the client request by carrying out its various life cycle methods and providing the necessary response back to tomcat as JSP pages. Tomcat then returns the response back to the client by rendering the JSP.

#### 参考来源

[Apache Tomcat](http://tomcat.apache.org/index.html)

[What is Apache Tomcat](https://www.educba.com/what-is-apache-tomcat/)