WARNING {-}
=======

Jaybird 3.0 is still in development. This version is provided for testing
purposes only. We'd appreciate your feedback, but we'd like to emphasize that
this version is **unstable** and **not ready for production**.

The protocol implementation has been fundamentally rewritten and changes have
been made for stricter JDBC conformance. As a result the driver might exhibit
different behavior than previous versions. Read these release notes carefully to
see if those differences are intentional. 

Bug reports about undocumented changes in behavior are appreciated. Feedback can
be sent to the Firebird-java mailing list or reported on the issue tracker
<http://tracker.firebirdsql.org/browse/JDBC>.

General Notes
=============

Jaybird is a JCA/JDBC driver suite to connect to Firebird database servers. 

This driver is based on both the JCA standard for application server
connections to enterprise information systems and the well-known JDBC standard.
The JCA standard specifies an architecture in which an application server can
cooperate with a driver so that the application server manages transactions,
security, and resource pooling, and the driver supplies only the connection
functionality. While similar to the JDBC XADataSource concept, the JCA
specification is considerably clearer on the division of responsibility between
the application server and driver.

About this version
------------------

Jaybird 3.0 is a big change from Jaybird 2.2 and earlier. We have rewritten the
entire low-level implementation to be able to support protocol improvements in 
newer Firebird versions, made changes with a stricter interpretation of the JDBC 
requirements, and removed some parts that were either obsolete or not 
functioning correctly.

We recommend that you do not consider Jaybird 3.0 a drop-in replacement for 
Jaybird 2.2, and study these release notes carefully. Test your product with
Jaybird 3.0 before using it in production.

Supported Firebird versions
---------------------------

Jaybird 3.0 was tested against Firebird 2.5.6, and 3.0.0, but should also 
support other Firebird versions from 2.0 and up.

Formal support for Firebird 1.x has been dropped (although in general we expect
the driver to work). The Type 2 and embedded server JDBC drivers use JNA to
access the Firebird client or embedded library.

This driver does not support InterBase servers due to Firebird-specific changes
in the protocol and database attachment parameters that are sent to the server.

Jaybird 3.0 is the last version to support Firebird 2.0.

Supported Java versions
-----------------------

Jaybird 3.0 supports Java 7 (JDBC 4.1) and Java 8 (JDBC 4.2). Support for 
earlier Java versions has been dropped.

Rudimentary support for Java 9 (JDBC 4.3) is available using the Java 8 version,
but real module support will not be available until Jaybird 3.1.

Jaybird 3.0 is the last version to support Java 7.

Specification support
---------------------

Jaybird supports the following specifications:

|Specification|Notes
|-------------|----------------------------------------------------------------
| JDBC 4.3    | Driver implements all JDBC 4.3 methods for features supported by Firebird; Java 9 supported using the Java 8 driver.
| JDBC 4.2    | Driver implements all JDBC 4.2 methods for features supported by Firebird.
| JDBC 4.1    | Driver implements all JDBC 4.1 methods for features supported by Firebird.
| JDBC 4.0    | Driver implements all JDBC 4.0 interfaces and supports exception chaining.
| JCA 1.0     | Jaybird provides implementation of `javax.resource.spi.ManagedConnectionFactory` and related interfaces. CCI interfaces are not supported. Although Jaybird depends on the JCA 1.5 classes, JCA 1.5 compatibility is currently not guaranteed.
| JTA 1.0.1   | Driver provides an implementation of `javax.transaction.xa.XAResource` interface via JCA framework and `XADataSource` implementation.
| JMX 1.2     | Jaybird provides a MBean to manage Firebird servers and installed databases via JMX agent.

Getting Jaybird 3.0
===================

Jaybird 3.0-alpha-1
-------------------

### Maven ###

Jaybird 3.0 Alpha 1 is available from Maven central:

Groupid: `org.firebirdsql.jdbc`,\
Artifactid: `jaybird-jdkXX` (where `XX` is `17` or `18`).\
Version: `3.0.0-alpha-1`

For example:

~~~ {.xml}
<dependency>
    <groupId>org.firebirdsql.jdbc</groupId>
    <artifactId>jaybird-jdk18</artifactId>
    <version>3.0.0-alpha-1</version>
</dependency>
~~~

If your application is deployed to a Java EE application server, you will need to
exclude the `javax.resource:connector-api` dependency, and add it as a provided 
dependency:

~~~ {.xml}
<dependency>
    <groupId>org.firebirdsql.jdbc</groupId>
    <artifactId>jaybird-jdk18</artifactId>
    <version>3.0.0-alpha-1</version>
    <exclusions>
        <exclusion>
            <groupId>javax.resource</groupId>
            <artifactId>connector-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>javax.resource</groupId>
    <artifactId>connector-api</artifactId>
    <version>1.5</version>
    <scope>provided</scope>
</dependency>
~~~

If you want to use Type 2 support (native, local or embedded), you need to 
explicitly include JNA 4.2.2 as a dependency:

~~~ {.xml}
<dependency>
    <groupId>net.java.dev.jna</groupId>
    <artifactId>jna</artifactId>
    <version>4.2.2</version>
</dependency>
~~~

We plan to make native and embedded support a separate library in future 
releases, and provide Firebird client libraries as Maven dependencies as well.

### Download ###

You can download the latest versions from <http://www.firebirdsql.org/en/jdbc-driver/>

At minimum Jaybird 3.0 requires `jaybird-3.0.0-alpha-1.jar` and 
`connector-api-1.5.jar`. You can also use `jaybird-full-3.0.0-alpha-1.jar` as it
includes the connector-api files.

If you deploy your application to a Java EE application server, then you must 
use `jaybird-3.0.0-alpha-1.jar` (not `-full`!), and **not** include 
`connector-api-1.5.jar` as this dependency will be provided by your application 
server.

For `getGeneratedKeys` support you will need to include 
`antlr-runtime-4.5.3.jar` on your classpath.

For native, local or embedded support, you will need to include `jna-4.2.2.jar` 
on your classpath.

Snapshot versions
-----------------

Occasionally we release a Jaybird 3 snapshot for testing purposes to
the [Sonatype OSS snapshot repository](https://oss.sonatype.org/content/repositories/snapshots/).

Groupid: `org.firebirdsql.jdbc`,\
Artifactid: `jaybird-jdkXX` (where `XX` is `17` or `18`).\
Version: `3.0.0-SNAPSHOT`

For example:

~~~ {.xml}
<dependency>
    <groupId>org.firebirdsql.jdbc</groupId>
    <artifactId>jaybird-jdk18</artifactId>
    <version>3.0.0-SNAPSHOT</version>
</dependency>
~~~

You need to add the Sonatype OSS snapshot repository to your maven repository config or pom.

Upgrading from Jaybird 2.2 to Jaybird 3.0
=========================================

Maven
-----

Upgrade the version of the dependency to 3.0.0-alpha-1. If you use native or 
embedded, you will also need to remove the `.dll` or `.so`, see the next 
section.

For more detailed instructions, see also the information on Maven in
[Getting Jaybird 3.0]. 

Manual install
--------------

If you manage your dependencies manually, you need to do the following:

1.  Replace the Jaybird library 2.2 with the 3.0
    - `jaybird-2.2.x.jar` with `jaybird-3.0.0-alpha-1.jar` 
    - `jaybird-full-2.2.x.jar` with `jaybird-full-3.0.0-alpha-1.jar`

2.  If installed, remove `antlr-runtime-3.4.jar` and replace it with 
    `antlr-runtime-4.5.3.jar`. This library is necessary for `getGeneratedKeys`
    support.
  
3.  If you use native (or embedded) you can remove the native library: 
    - `jaybird22.dll`, 
    - `libjaybird22.so`, 
    - `jaybird22_x64.jar` or
    - `libjaybird22_x64.so`
    
    Instead you need to add `jna-4.2.2.jar` to the classpath of your 
    application. This library is necessary for native, local and embedded 
    support.
    
Gotcha's
--------

### Wildfly ###

When you use Jaybird 3 in Wildfly (or JBoss), you will need to add the module 
`javax.xml.bind.api` to your module to get it to work.

The minimal `module.xml` to use Jaybird 3 under Wildfly is:

``` {.xml}
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.0" name="org.firebirdsql">
  <resources>
    <resource-root path="jaybird-3.0.x.jar"/>
  </resources>
  <dependencies>
    <module name="javax.api"/>
    <module name="javax.transaction.api"/>
    <module name="javax.resource.api"/>
    <module name="javax.xml.bind.api"/> <!-- Add this -->
  </dependencies>
</module>
```

What's new in Jaybird 3.0
=========================

Java support
------------

### Java 6 ###

Support for Java 6 has been dropped.

### Java 7 and 8 ###

The driver supports Java 7 and 8 and provides improved support for JDBC 4.2
features.

The improved support includes

-   Support for `java.time` in `set/getObject`
-   Support for `getObject(int/String, Class<?>)`
-   Support for `setBinaryStream`/`setCharacterStream` with no length or 
    (long) length beyond `Integer.MAX_VALUE`
-   Support for large update counts (but not `setLargeMaxRows`)

### Java 9 ###

Jaybird currently does not formally support Java 9 (JDBC 4.3), although some of
the JDBC 4.3 features have been implemented. 

You can use the Java 8 driver under Java 9, but it is necessary to add the
`java.xml.bind` module using `-addmods java.xml.bind`.

Firebird support
----------------

Support for Firebird 1.0 and 1.5 has been dropped. See [Firebird 1.0 and 1.5 no
longer supported] for details.

Firebird 2.1 support is improved with the implementation of wire protocol
version 11.

Firebird 2.5 support is improved with the implementation of wire protocol
version 12.

Firebird 3.0 support is improved with the (partial) implementation of wire
protocol 13 and support for the _Srp_ authentication plugin. Version 13 support
does not yet provide Firebird 3.0 wire encryption and zlib compression. Wire
encryption is planned for Jaybird 3.1. Support for zlib compression is not 
planned yet.

See also [Jaybird and Firebird 3](https://github.com/FirebirdSQL/jaybird/wiki/Jaybird-and-Firebird-3)
on the wiki.

Support for protocol version 13 and the SRP authentication was contributed
by [Hajime Nakagami](https://github.com/nakagami).

### Other Firebird feature support ###

*   Add support for streaming backup and restore ([JDBC-256](http://tracker.firebirdsql.org/browse/JDBC-256))

    This feature was contributed by [Ivan Arabadzhiev](https://github.com/ls4f)

New low-level implementation
----------------------------

Jaybird 3.0 has a substantially rewritten low-level implementation (the wire
protocol and native implementation) and a number of changes for JDBC 
conformance.

The rewrite of the low-level implementation was prompted by the new
authentication (and wire encryption) in Firebird 3.0 (protocol version 13), and
the fact that other improvements in the Firebird wire protocol (versions 11 
and 12) were not yet available in the pure Java implementation in Jaybird. The
old implementation of the wire protocol did not lend itself for - easily - 
supporting multiple protocol versions.

The new low-level implementation also means that the old GDS API 
(`org.firebirdsql.gds.GDS`) has been removed and is no longer available.

For the native, local and embedded support the use of 
`jaybirdxx.dll`/`libjaybirdxx.so` is no longer necessary. For more details, see
[Type 2 (native) and embedded driver].

Support for java.util.logging added
-----------------------------------

We have added support for `java.util.logging`, and made it the default logging
implementation.

We have applied the following mapping for the log levels:

* `Logger.trace`: `Level.FINER`
* `Logger.debug`: `Level.FINE`
* `Logger.info`: `Level.INFO`
* `Logger.warn`: `Level.WARNING`
* `Logger.error`: `Level.SEVERE`
* `Logger.fatal`: `Level.SEVERE`

We have also added some options to control logging behavior:

*   System property `org.firebirdsql.jdbc.disableLogging` with value `true`
    will disable logging entirely.
    
*   System property `org.firebirdsql.jdbc.forceConsoleLogger` with value `true`
    will force logging to the `System.out` for info and lower and `System.err` 
    for warn and above (`debug` and `trace` are disabled in the implementation).
    
*   System property `org.firebirdsql.jdbc.loggerImplementation` to specify an
    alternative implementation of the interface `org.firebirdsql.logging.Logger`.
    
    This implementation must be public and must have a public constructor with
    a single `String` argument for the logger name. See also 
    [Support for log4j 1.x removed] for an example.
    
    The `org.firebirdsql.logging.Logger` interface is volatile and might change 
    in future minor releases (but not point/bugfix releases).

Support for log4j 1.x removed
-----------------------------

Support for Log4J 1.x has been removed. If you really need it, you can implement
the interface `org.firebirdsql.logging.Logger` with a public constructor 
accepting a single `String` parameter (the logger name).

To instruct Jaybird to use this logger implementation, specify the system 
property `org.firebirdsql.jdbc.loggerImplementation` with the class name.

Say you have created the following implementation

``` {.java}
package org.example.jaybird.logging;

public class Log4jLogger implements org.firebirdsql.logging.Logger {
    public Log4jLogger(String name) {
        // create the logger    
    }
    // implementation of interface
}
```

You will need to specify:

    -Dorg.firebirdsql.jdbc.loggerImplementation=org.example.jaybird.logging.Log4jLogger

Potentially breaking changes
----------------------------

Jaybird 3.0 contains a number of changes that might break existing applications.

See also [Compatibility changes] for details.

### Specifying connection character set is now required ###

Jaybird 3.0 requires you to specify the connection character set by either
specifying `encoding=<firebird encoding>` or `charSet=<Java encoding>`.

For more information see: [Connection rejected without an explicit character set]

### ANTLR 4 runtime ###

The generated keys functionality now requires ANTLR 4.5. Make sure to replace
`antlr-runtime-3.4.jar` with the `antlr-runtime-4.5.3.jar` included in the 
distribution zip. If you use maven this will happen automatically.

As in previous versions: if the ANTLR runtime is not on the classpath, then 
the generated keys functionality will not be available.

### Handling of character set OCTETS ###

Columns of type `CHAR` and `VARCHAR` with character set `OCTETS` are now handled
as JDBC type `BINARY` and `VARBINARY`, respectively ([JDBC-240](http://tracker.firebirdsql.org/browse/JDBC-240))

See also [Character set OCTETS handled as JDBC (VAR)BINARY].

### Changes to character set handling ###

We reimplemented the character set handling in Jaybird which may lead to
different behavior, especially when using `NONE` as the connection character set.

See also [Character set handling].

Other fixes and changes
-----------------------

*   Fix: IP-address is reversed on big-endian platforms ([JDBC-98](http://tracker.firebirdsql.org/browse/JDBC-98))

*   Improved support of JDBC Escape syntax (`{...}`) and supported functions (*TODO: Provide more info?*)
    ([JDBC-223](http://tracker.firebirdsql.org/browse/JDBC-223))

    The escape parser will now only allow the function names defined in
    Appendix D of the JDBC specification (4.1 for now). For unsupported
    functions or functions not listed in Appendix D a `FBSQLParseException` will
    be thrown.
    
    The database metadata will now correctly report supported functions for
    `getNumericFunctions`, `getStringFunctions`, `getSystemFunctions` and 
    `getTimeDateFunctions`.
    
*   Nested JDBC escapes are now supported ([JDBC-292](http://tracker.firebirdsql.org/browse/JDBC-292))

*   Changed locking to coarser blocks with - as far as possible - a single lock
    object per connection for all connection-derived objects ([JDBC-435](http://tracker.firebirdsql.org/browse/JDBC-435))

    This should prevent deadlocks on concurrent access. In some cases locks
    were obtained in different order (eg (statement, connection), and
    (connection, statement)). The downside is reduced concurrency, but as using
    a connection from multiple threads concurrently is discouraged anyway, that
    is an acceptable price to pay.

*   `DatabaseMetaData.getColumns` will report `YES` for `IS_AUTOINCREMENT` and 
    `IS_GENERATEDCOLUMN` if the column is an identity column ([JDBC-322](http://tracker.firebirdsql.org/browse/JDBC-322))
     
*   Added Jaybird-specific columns `JB_IS_IDENTITY` and `JB_IDENTITY_TYPE` to 
    `DatabaseMetaData.getColumns` to report the type of identity column ([JDBC-322](http://tracker.firebirdsql.org/browse/JDBC-322))
    
    `JB_IS_IDENTITY` has either `YES` or `NO` as possible values and can be used
     to check if the column is really an identity column.

    Possible values for `JB_IDENTITY_TYPE` are:
    
    * `null` : not an identity column, or unknown identity type, 
    * `ALWAYS` : a `GENERATED ALWAYS AS IDENTITY` column (NOTE: not yet 
      supported by Firebird),
    * `BY DEFAULT` : a `GENERATED BY DEFAULT AS IDENTITY` column.
    
    You should always retrieve these columns by name, as their position will 
    change when the JDBC specification adds new columns.

*   Added field index to `DataTruncation` exceptions ([JDBC-405](http://tracker.firebirdsql.org/browse/JDBC-405))

Removal of deprecated classes and packages
------------------------------------------

See [Removal of deprecated classes, packages and methods] in 
[Compatibility changes] for more details.

Known Issues
============

There are currently no known issues.

Compatibility changes
=====================

Jaybird 3.0 introduces some changes in compatibility and announces future
breaking changes.

The changes due to the new protocol implementation and/or JDBC conformance are
listed below.

**The list is not yet complete, if you notice a difference in behavior that is
not listed, please report it as bug.** It might have been a change we forgot to
document, but it could just as well be an implementation bug.

Character set handling
----------------------

### Character set OCTETS handled as JDBC (VAR)BINARY ###

Columns of type `CHAR(n) CHARACTER SET OCTETS` and
`VARCHAR(n) CHARACTER SET OCTETS` are now handled as JDBC type
`java.sql.Types.BINARY` and `java.sql.Types.VARBINARY`, respectively.

The connection property `octetsAsBytes` no longer has any effect, metadata and
usage will always be `(VAR)BINARY`.

With this change the getters (on result set/callable statement) and
setters (prepared/callable statement) and update methods (result set) for columns
of this type have been restricted to:

* `set/get/updateNull`
* `get/set/updateBytes`
* `get/set/updateBinaryStream`
* `get/set/updateAsciiStream`
* `get/set/updateString` (using the default encoding or connection encoding)
* `get/set/updateCharacterStream` (using the default encoding or connection encoding)
* `get/set/updateObject` (with `String`, `byte[]`, `InputStream`, `Reader`)

Other getters/setters/updaters or object types supported for
'normal' `(VAR)CHAR` fields are not available.

### Connection character set NONE ###

Jaybird will now use the `(VAR)CHAR` or `BLOB SUB_TYPE TEXT` character set
information for decoding. This means that when using connection character set 
`NONE` that columns that have an explicit character set will be decoded and
encoded using that character set instead of the platform default encoding (or 
explicitly specified Java character set when specifying both `encoding=NONE` 
**and** `charSet=<some java charset>`).

This may lead to unexpected character conversions if - for example - you have 
always been reading and writing `Cp1251` data from a `WIN1252` column: it will
now be read as `Cp1252`. You will need to convert the column and data to the 
right character set.

### Connection rejected without an explicit character set ###

If no explicit character set has been set, Jaybird 3.0 will reject the 
connection with an `SQLNonTransientConnectionException` with message 
_"Connection rejected: No connection character set specified (property lc_ctype,
encoding, charSet or localEncoding). Please specify a connection character set 
(eg property charSet=utf-8) or consult the Jaybird documentation for more 
information."_ ([JDBC-446](http://tracker.firebirdsql.org/browse/JDBC-446))

In Jaybird 2.2 and earlier, Jaybird would default to connection character set 
`NONE` if no character set had been specified (through `lc_ctype`/`encoding` 
and/or `charSet`/`localEncoding`). This can result in incorrect character set
handling when the database is used from different locales.

To address this change, explicitly set the connection character set using
one of the following options:

*   Use connection property `encoding` (or `lc_ctype`) with Firebird character
    set names. 
    
    Use `encoding=NONE` for the 'old' default behavior (with some caveats, see 
    other sections).

*   Use connection property `charSet` (or `localEncoding`) with Java character
    set names.

*   By providing a default character set with system property 
    `org.firebirdsql.jdbc.defaultConnectionEncoding`. Jaybird will apply the
    specified character set as the default when none is specified.
    
    This property only supports Firebird character set names. The property must
    be set on start up (or at least before Jaybird-related classes get loaded). 

    Use `-Dorg.firebirdsql.jdbc.defaultConnectionEncoding=NONE` to revert to the
    old behavior (with some caveats, see other sections).

Logging
-------

Support for log4j has been removed, we now default to `java.util.logging`. The
previous default was no logging.

See also [Support for java.util.logging added] and [Support for log4j 1.x removed].

Exceptions
----------

*   `FBSQLException` and sub-classes replaced with actual `java.sql.*` exceptions.

    Over time the JDBC exception hierarchy has become more complicated with more
    specific exceptions. It was easier to use the `java.sql` exception-
    hierarchy, than to duplicate the hierarchy within Jaybird.

    This change does not mean that there are no Firebird-specific `SQLException`
    sub-classes anymore, but in general we strive to use the standard
    exceptions where possible.

*   Class `FBSQLWarning` has been removed and replaced with `SQLWarning`.

*   Methods with `throws FBSQLException` changed to `throws SQLException`

    As we are preferring the standard exceptions, the throws clause has been
    widened to `SQLException`. Note that most methods already had
    `throws SQLException`, so the impact is limited. This change specifically
    impacts:

    * `org.firebirdsql.jdbc.FirebirdPreparedStatement`
        * `getExecutionPlan()`
        * `getStatementType()`
    * `org.firebirdsql.management.FBServiceManager`
        * `executeServicesOperation`
    * A number of classes/methods internal to the Jaybird implementation

*   `org.firebirdsql.gds.GDSException` has been removed from exception causes.

    The new low-level implementation throws `java.sql.SQLException` classes
    eliminating the need for `GDSException` (which was usually set as the
    `cause` of an `SQLException`). In some cases uses of `GDSException`
    have been replaced by `org.firebirdsql.jdbc.FBSQLExceptionInfo` to
    report exception message elements and their error codes.

*   Exception message format changed:
   
    *   Exception message elements are now separated by semi-colon, not by
        linebreak.

        Errors reported by Firebird can consist of multiple elements. In Jaybird
        2.2 and earlier the final exception message was constructed by
        separating these elements by a linebreak. These elements are now
        separated by a semi-colon and a space.

    *   Exception message now reports SQLState and error code.

    For example, a "Table unknown" (error 335544580) in Jaybird 3.0 has message:

    ~~~
    Dynamic SQL Error; SQL error code = -204; Table unknown; TABLE_NON_EXISTENT; At line 1, column 13 [SQLState:42S02, ISC error code:335544580]
    ~~~

    Jaybird 2.2 and earlier reported this as (`\n` added to show line break):

    ~~~
    Dynamic SQL Error\n
    SQL error code = -204\n
    Table unknown\n
    TABLE_NON_EXISTENT\n
    At line 1, column 13
    ~~~

    The SQLState and error code are only included in the message if the exception
    is constructed using `FbExceptionBuilder` (for example errors received from
    Firebird). Some parts of the code construct an `SQLException` directly,
    those messages do not contain the SQLState and error code in the message. We
    strive to improve that in future versions.

*   More specific error reported by `SQLException.getErrorCode` and
    `SQLException.getSQLState`.

    In previous versions a large class of errors always reported error 335544569
    ("Dynamic SQL Error" or `isc_dsql_error`) with SQLState 42000, Jaybird now
    tries to find a more specific error code (and SQLState) in the status vector.

Firebird 1.0 and 1.5 no longer supported
----------------------------------------

Support for Firebird 1.0 and 1.5 has been dropped in Jaybird 3.0. In general we
expect the driver to remain functional, but chances are certain metadata (eg 
`DatabaseMetaData`) will break if we use features introduced in newer versions.

In general we will no longer fix issues that only occur with Firebird 1.5 or
earlier.

Java 5 and 6 no longer supported
--------------------------------

Support for Java 6 (JDBC 4.0) has been dropped in Jaybird 3.0. The Jaybird 3.0
sources no longer compile with Java 6 due to use of Java 7 language features and
JDBC 4.1 specific features.

Support for Java 5 was already removed with Jaybird 2.2.8.

Stricter JDBC compliance
------------------------

In Jaybird 3.0 a number of changes were made for stricter compliance to the JDBC
specification.

### Statement ###

_Unless explicitly indicated, changes also apply to `PreparedStatement` and
`CallableStatement`_

*   Generated keys `ResultSet` only available through `getGeneratedKeys`.

    The generated keys `ResultSet` from a statement is no longer available
    through `getResultSet`, but only through `getGeneratedKeys` as the JDBC
    specification does not consider the generated keys `ResultSet` a normal
    `ResultSet`.

    <span id="generated-query-types">This applies to statements executed (or
    prepared) using:</span>

    * `Statement.execute(String, int)` or `Statement.executeUpdate(String, int)`
      with value `Statement.RETURN_GENERATED_KEYS`,
    * `Statement.execute(String, int[])` or
      `Statement.executeUpdate(String, int[])`,
    * `Statement.execute(String, String[])` or
      `Statement.executeUpdate(String, String[])`,
    * `Connection.prepareStatement(String, int)` with value
      `Statement.RETURN_GENERATED_KEYS`,
    * `Connection.prepareStatement(String, int[])`,
    * `Connection.prepareStatement(String, String[])`.

    This change does not apply to executing `INSERT ... RETURNING ...` as a
    normal statement.

*   Update count immediately available after executing generated keys queries.

    Previously the update count of a generated keys query was only available
    after calling `getMoreResults` followed by a call to `getUpdateCount`. This
    change means that `executeUpdate` will now correctly return the update count
    (usually `1`) instead of `-1`. The same applies to calling `getUpdateCount`
    after `execute` (without the need to call `getMoreResults`).

    For the definition of generated keys queries see [the previous item](#generated-query-types).

*   Use of function escapes (`{fn ...}`) not defined in the JDBC standard will 
    now throw an `FBSQLParseException`, previously the escape was removed and 
    the function was used as is.

### PreparedStatement ###

_Unless explicitly indicated, changes also apply to `CallableStatement`_

*   Method `setUnicodeStream` now always throws
    an `SQLFeatureNotSupportedException`. The previous implementation did not
    conform to the (deprecated) JDBC requirements and instead behaved like
    `setBinaryStream`.

    For the behavior in Jaybird 2.2 and earlier, use `setBinaryStream`.
    Otherwise use `setCharacterStream`.

*   Methods `setNString`, `setNClob`, and `setNCharacterStream` will now behave
    as their counterpart without `N` (ie `setString`, `setClob`, and
    `setCharacterStream`)

    This implementation is not compliant with the JDBC requirements for
    `NVARCHAR/NCHAR/NCLOB` support, it is only provided for compatibility
    purposes.

### CallableStatement ###

*   Methods `getNString`, `getNClob`, and `getNCharacterStream` will now behave
    as their counterpart without `N` (ie `getString`, `getClob`, and
    `getCharacterStream`)

    This implementation is not compliant with the JDBC requirements for
    `NVARCHAR/NCHAR/NCLOB` support, it is only provided for compatibility
    purposes.

### ResultSet ###

*   Method `getUnicodeStream` now always throws
    an `SQLFeatureNotSupportedException`. The previous implementation did not
    conform to the (deprecated) JDBC requirements and instead behaved like
    `getBinaryStream`.

    For the behavior in Jaybird 2.2 and earlier, use `getBinaryStream`.
    Otherwise use `getCharacterStream`.

*   Methods `getNString`, `updateNString`, `getNClob`, `updateNClob`,
    `getNCharacterStream`, and `updateNCharacterStream` will now behave as their
    counterpart without `N` (ie `getString`, `updateString`, `getClob`,
    `updateClob`, `getCharacterStream`, and `updateCharacterStream`)

    This implementation is not compliant with the JDBC requirements for
    `NVARCHAR/NCHAR/NCLOB` support, it is only provided for compatibility
    purposes.

### Clob ###

*   The `Clob` implementations of the driver now also implement `NClob` so they
    can be returned from `getNClob`.

    This implementation is not compliant with the JDBC requirements for
    `NVARCHAR/NCHAR/NCLOB` support, it is only provided for compatibility
    purposes.

### DatabaseMetaData ###

#### Pattern parameters ####

The `java.sql.DatabaseMetaData` implementation has been changed to follow the
JDBC requirements for object name pattern or object name parameters (referred to
as _patterns_ in the rest of this section).

These changes affect all methods that accept one or more pattern or name
parameters and return a `ResultSet`. This includes, but is not limited to,
`getTables` and `getColumns`.

In Firebird unquoted object names are stored upper case, while quoted object
names are stored as is. The JDBC specification states that the pattern must
match the object name as stored in the database. This means the pattern should
be case sensitive.

The changes made are as follows:

*   `null` will always be interpreted as `"%"` (before this rule was applied
    inconsistently)
*   Empty string will no longer match (ie they are no longer interpreted as
    `"%"`) unless explicitly allowed by the method javadoc (usually only the
    `catalogPattern` and `schemaPattern`, which are always ignored by Jaybird)
*   Double quotes around a pattern will no longer be stripped, and therefor will
    now never match existing object names
*   The driver will no longer try the uppercase variant of the provided
    pattern(s) if the original value(s) did not yield a result
*   Object name parameters that are not patterns (as indicated by the absence of
    `Pattern` in the parameter name) will no longer have backslashes removed

Review your `DatabaseMetaData` usage and make the following changes:

*   Empty string parameters: replace with `"%"` (or `null`)
*   Double quotes around patterns: remove the double quotes
*   Casing of patterns should be reviewed for correctness
*   Non-pattern parameters containing `\` should be reviewed for correctness

Some examples:

~~~ {.sql}
CREATE TABLE tablename ( -- tablename is stored as TABLENAME in metadata
   column1 INTEGER,      -- column1 is stored as COLUMN1 in metadata
   "column2" INTEGER     -- "column2" is stored as column2 in metadata
);
~~~

In Jaybird 2.2 using `getColumns(null, null, "tablename", "column%")` returns
`COLUMN1`(!). In Jaybird 3.0 this produces **no rows** as `tablename` does not
match `TABLENAME`.

Changing the query to `getColumns(null, null, "TABLENAME", "column%")` in
Jaybird 2.2 and 3.0 produces only one row (`column2`), as `COLUMN1` does not
match `column%`.

In Jaybird 2.2 using `getColumns(null, null, "\"TABLENAME\"", "column%")`
returns `column2`, in Jaybird 3.0 this produces **no rows** as quotes are no
longer stripped.

In Jaybird 2.2 using `getColumns(null, null, "TABLENAME", "")` returns all
columns of the table, in Jaybird 3.0 this produces **no rows** as empty string
does not match any column. Instead, you should use
`getColumns(null, null, "TABLENAME", "%")`.

#### getTables ####

Apart from the change described above, the following has changed for `getTables`

*   The result set is now sorted by `TABLE_TYPE` and then by `TABLE_NAME` as
    required by the JDBC API doc, previously we only only sorted on
    `TABLE_NAME`.
*   Support for table type `"GLOBAL TEMPORARY"` added for databases with
    ODS 11.2 or higher (Firebird 2.5 or higher). In previous versions,
    the global temporary tables were reported as normal tables (type `"TABLE"`)

**TODO: Add other changes**

Removal of old GDS API
----------------------

The old GDS API (`org.firebirdsql.gds.GDS`) and its implementations have been
removed. This removal includes a number of related classes and methods. Classes
and interfaces in the `org.firebirdsql.gds` package and sub-packages that still
exist may have moved to other packages, or may have a changed API.

In general we advise you to not use the low-level implementation directly. If
you see something that is only possible through the low-level API, please file
an improvement ticket or create a pull request to add it to the JDBC or
management classes.

Type 2 (native) and embedded driver
-----------------------------------

Jaybird no longer needs a `jaybirdxx.dll` or `libjaybirdxx.so` for the Type 2 
and embedded driver. Jaybird now uses JNA to access the client library.

If you want to use the Type 2 driver, or Firebird embedded, then you need to
include the `jna-4.2.2.jar` on the classpath.

When using Maven, you need to specify the dependency on JNA yourself, as we 
don't depend on it by default:

``` {.xml}
<dependency>
    <groupId>net.java.dev.jna</groupId>
    <artifactId>jna</artifactId>
    <version>4.2.2</version>
</dependency>
```

The `fbclient.dll`, `fbembed.dll`, `libfbclient.so`, or `libfbembed.so` need to
be on the path, or the location needs to be specified in the system property 
`jna.library.path`.

In the future we will move the Type 2 support to a separate library and provide 
JNA-compatible jars that provide the native libraries of a specific Firebird 
version.

**TODO: May need further documentation**

Removal of deprecated classes, packages and methods
---------------------------------------------------

### DataSource and connection pooling ###

The classes in `org.firebirdsql.pool` and `org.firebirdsql.pool.sun` have been
removed completely, with the exception of 
`org.firebirdsql.pool.FBSimpleDataSource`. This class has been moved to
`org.firebirdsql.ds.FBSimpleDataSource`. A subclass with the same name is kept
in `org.firebirdsql.pool` for backwards compatibility. This subclass will be
removed in Jaybird 3.1.

With this change, there are no `javax.sql.DataSource` implementations in Jaybird
that provide connection pooling (the `javax.sql.ConnectionPoolDataSource`
implementations are for use by a connection pool and not a connection pool
themselves). Either use the connection pool provided by your application server,
or use a third-party connection pool like c3p0, Apache DBCP or HikariCP.

The class `org.firebirdsql.jca.FBXADataSource` has been removed as well. Its
replacement is `org.firebirdsql.ds.FBXADataSource` (which was introduced in
Jaybird 2.2).

### FirebirdSavepoint ###

All method definitions in the interface 
`org.firebirdsql.jdbc.FirebirdSavepoint` were removed, and methods referencing
this interface in `org.firebirdsql.jdbc.FirebirdConnection` have been removed as
the interface duplicated the `java.sql.Savepoint` interface and related methods
in `java.sql.Connection`. The interface itself remains for potential future
Firebird-specific extensions.

### Reducing visibility of implementation ###

The following classes, interfaces and/or methods had their visibility reduced as
they are implementation artifacts, and should not be considered API:

- `org.firebirdsql.jdbc.FBDriverPropertyManager` (to package private)

### FBMaintenanceManager ###

In `FBMaintenanceManager` the following changes have been made:

- `getLimboTransactions()` will return `long[]` instead of `int[]`
- `limboTransactionsAsList()` will return `List<Long>` instead of `List<Integer>`
- `getLimboTransactionsAsLong()` (introduced in 2.2.11) has been removed in 
  favor of `getLimboTransactions()`
- `limboTransactionsAsLongList` (introduced in 2.2.11) has been removed in favor
  of `limboTransactionsAsList`

These methods were previously not defined in the `MaintenanceManager` interface.

Miscellaneous
-------------

-   In some cases `ResultSetMetaData.getPrecision` will estimate the precision.
    In Jaybird 2.2 and earlier the estimate used for `NUMERIC` and `DECIMAL` was
    `19`, this has been revised to the more correct value of `18`.
-   `ResultSetMetaData.getColumnDisplaySize` was revised to take into account 
    space for sign and decimal separator for numeric types.

Breaking changes for Jaybird 3.1
--------------------------------

With Jaybird 3.1 the following breaking changes will be introduced.

### Dropping support for Firebird 2.0 ###

Jaybird 3.1 will drop support for Firebird 2.0. In general we expect the driver
to remain functional, but chances are certain metadata (eg `DatabaseMetaData`)
will break if we use features introduced in newer versions.

### Dropping support for Java 7 ###

Jaybird 3.1 will very likely drop support for Java 7 (this decision is not final yet).

### Removal of deprecated methods ###

The following methods will be removed in Jaybird 3.1:

-   `CharacterTranslator.getMapping()`, use `CharacterTranslator.getMapping(char)`
    instead.
-   `GDSHelper.iscVaxInteger(byte[] buffer, int pos, int length)` use
    `VaxEncoding.iscVaxInteger(byte[] buffer, int startPosition, int length)`
    instead.
-   `GDSHelper.iscVaxLong(byte[] buffer, int pos, int length)` use
    `VaxEncoding.iscVaxLong(byte[] buffer, int startPosition, int length)`
    instead.
-   `MaintenaceManager.commitTransaction(int transactionId)`, use
    `MaintenanceManager.commitTransaction(long transactionId)` instead.
-   `MaintenaceManager.rollbackTransaction(int transactionId)`, use
    `MaintenanceManager.rollbackTransaction(long transactionId)` instead.

### Removal of deprecated constants ###

The following constants will be removed in Jaybird 3.1:

-   All `SQL_STATE_*` constants in `FBSQLException`,
    `FBResourceTransactionException`, `FBResourceException`, and
    `FBDriverNotCapableException` will be removed. Use equivalent constants in
    `org.firebirdsql.jdbc.SQLStateConstants`.