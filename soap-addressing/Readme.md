Introduction
============
This quickstart demonstrates the usage of SOAP with WS-Addressing. It binds
three SwitchYard services over SOAP/HTTP URL. There are two flows to this quickstart
as illustrated below. When the client sends a WS-A request, a reply is immediately sent
back to the client from the OrderService. The OrderService has now forwarded the
request to WarehouseService with a ReplyTo address set as ClientService. Once the
WarehouseService completes its processing the response is sent to ClientService.
The client service then again writes the response to a file.

<pre>
+-----------------+      +--------------+      +-------------+      +------------------+
| http://         | ---- | OrderService | ---- | camel:route | ---- | WarehouseService |
+-----------------+      +--------------+      +-------------+      +------------------+

+------------------+      +---------------+      +-------------+
| WarehouseService | ---- | ClientService | ---- | camel:file  |
+------------------+      +---------------+      +-------------+

</pre>

![SOAP with WS-Addressing](https://github.com/jboss-switchyard/quickstarts/raw/master/soap-binding-rpc/soap-addressing.jpg)

Running the quickstart
======================

JBoss AS 7
----------
1. Build the quickstart:
<pre>
    mvn clean install
</pre>
2. Start JBoss AS 7 in standalone mode:
<pre>
    ${AS}/bin/standalone.sh --server-config=standalone.xml
</pre>
3. If on Windows, please create a directory called 'tmp' under c:
4. Deploy the quickstart
<pre>
    mvn jboss-as:deploy
</pre>
5. Open a console window and type
<pre>
    mvn exec:java -Dexec.args="Boeing 10"
</pre>
6. You should see the following output
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
    <SOAP-ENV:Header>
        <Action xmlns="http://www.w3.org/2005/08/addressing">urn:switchyard-quickstart:soap-addressing:1.0:OrderService:orderResponse</Action>
        <MessageID xmlns="http://www.w3.org/2005/08/addressing"><some-unique-id></MessageID>
        <RelatesTo xmlns="http://www.w3.org/2005/08/addressing">uuid:3d3fcbbb-fd43-4118-b40e-62577894f39a</RelatesTo>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <orderResponse xmlns="urn:switchyard-quickstart:soap-addressing:1.0">
            <return>Thank you for your order. You should hear back from our WarehouseService shortly!</return>
        </orderResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
7. After a few seconds, this also should be displayed

Order Boeing with quantity 10 accepted.

## Further Reading

1. [SOAP Binding Documentation](https://docs.jboss.org/author/display/SWITCHYARD/SOAP)
2. [WS-Addressing] http://www.w3.org/standards/techs/wsaddr#w3c_all
