# Parsing XML with Python

[![Bright Data Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com/)

Learn how to parse XML in Python using libraries like ElementTree, lxml, and SAX to enhance your data processing projects.

- [Key Concepts of an XML File](#key-concepts-of-an-xml-file)
- [Various Ways to Parse XML in Python](#various-ways-to-parse-xml-in-python)
- [ElementTree](#elementtree)
- [lxml](#lxml)
- [minidom](#minidom)
- [SAX Parser](#sax-parser)
- [untangle](#untangle)

## Key Concepts of an XML File

Before diving into how to parse XML in Python, it's important to first understand what XML Schema Definition (XSD) is and the fundamental elements that make up an XML file. This foundational knowledge will guide you in choosing the right Python library for your parsing needs.

[XSD](https://en.wikipedia.org/wiki/XML_Schema_(W3C)) is a schema specification that defines the structure, content, and data types permitted in an XML document. It acts as a set of validation rules, ensuring that XML files follow a consistent format.

An XML file typically contains elements such as `Namespace`, `root`, `attributes`, `elements`, and `text content`, which together represent structured data.

- **[`Namespace`](https://www.w3schools.com/xml/xml_namespaces.asp)** uniquely identifies elements and attributes in XML documents. It helps prevent naming collisions and supports interoperability between different XML datasets.
- **[`root`](https://en.wikipedia.org/wiki/Root_element)** is the top-level element of an XML document. It serves as the entry point to the XML structure and encompasses all other elements.
- **[`attributes`](https://www.w3schools.com/xml/xml_attributes.asp)** offer additional context about an element. Defined within an element's start tag, they consist of name-value pairs.
- **[`elements`](https://www.w3schools.com/xml/xml_elements.asp)** are the core units of an XML file, representing the actual data or structural tags. Elements can nest within each other to build a hierarchy.
- **[`text content`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)** refers to the actual textual data between an element’s start and end tags. This can include plain text, numeric values, or other characters.

For instance, the [Bright Data sitemap](https://brightdata.com/post-sitemap.xml) follows this XML structure:

- **`urlset`** serves as the `root` element.
- **`<urlset xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">`** is the namespace declaration for the `urlset` element. This indicates that the schema rules apply to `urlset` and all nested elements.
- **`url`** is a direct child of the `root` element.
- **`loc`** is a child element within the `url` element.

Now that you’ve got a clearer picture of XSD and XML structure, it’s time to put that knowledge to use by parsing an XML file using a few helpful Python libraries.

## Various Ways to Parse XML in Python

Let's use Bright Data sitemap. In the following examples, the Bright Data sitemap content is fetched using the Python `requests` library.

The Python requests library is not built-in, so you need to install it before proceeding. You can do so using the following command:

```sh
pip install requests
```

## ElementTree

The [ElementTree XML API](https://docs.python.org/3/library/xml.etree.elementtree.html) offers a straightforward and user-friendly way to parse and generate XML data in Python. Since it’s part of Python’s standard library, there’s no need for any additional installation.

For instance, you can use the [`findall()`](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element.findall) method to retrieve all `url` elements from the root and print the text content of each `loc` element, like so:

```python
import xml.etree.ElementTree as ET
import requests

url = 'https://brightdata.com/post-sitemap.xml'

response = requests.get(url)
if response.status_code == 200:
   
    root = ET.fromstring(response.content)

    for url_element in root.findall('.//{http://www.sitemaps.org/schemas/sitemap/0.9}url'):
        loc_element = url_element.find('{http://www.sitemaps.org/schemas/sitemap/0.9}loc')
        if loc_element is not None:
            print(loc_element.text)
else:
    print("Failed to retrieve XML file from the URL.")

```

All the URLs in the sitemap are printed in the output:

```
https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

ElementTree is a simple and intuitive XML parser in Python, great for small scripts like reading RSS feeds. However, it lacks strong schema validation and may not be suitable for complex or large-scale XML parsing—libraries like `lxml` are better for those cases.

## lxml

[lxml](https://lxml.de/) is a fast, easy-to-use, and feature-rich API for parsing XML files in Python. You can [install `lxml`](https://lxml.de/installation.html#installation) using `pip`:

```sh
pip install lxml
```

Once installed, you can use `lxml` to parse XML files using [various API](https://lxml.de/apidoc/lxml.html) methods, such as `find()`, `findall()`, `findtext()`, `get()`, and `get_element_by_id()`.

For instance, you can use the `findall()` method to iterate over the `url` elements, find their `loc` elements (which are child elements of the `url` element), and then print the location text using the following code:

```python
from lxml import etree
import requests

url = "https://brightdata.com/post-sitemap.xml"

response = requests.get(url)
if response.status_code == 200:

    root = etree.fromstring(response.content)
    

    for url in root.findall(".//{http://www.sitemaps.org/schemas/sitemap/0.9}url"):
        loc = url.find("{http://www.sitemaps.org/schemas/sitemap/0.9}loc").text.strip()
        print(loc)
else:
    print("Failed to retrieve XML file from the URL.")
```

The output displays all the URLs found in the sitemap:

```
https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

Up to this point, you’ve seen how to locate elements and display their values. Next, let’s look at how to validate an XML file against its schema before parsing. This step confirms that the file follows the structure defined in the XSD.

Here’s what the sitemap’s XSD looks like:

```xml
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://www.sitemaps.org/schemas/sitemap/0.9"
           xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
           elementFormDefault="qualified"
           xmlns:xhtml="http://www.w3.org/1999/xhtml">

  
  <xs:element name="urlset">
    <xs:complexType>
      <xs:sequence>
        <xs:element ref="url" minOccurs="0" maxOccurs="unbounded"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  
  <xs:element name="url">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="loc" type="xs:anyURI"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>

</xs:schema>
```

To use the sitemap for schema validation, make sure you copy it manually and create a file named `schema.xsd`.

Now, validate the XML file using this XSD:

```python
from lxml import etree

import requests

url = "https://brightdata.com/post-sitemap.xml"

response = requests.get(url)

if response.status_code == 200:

    root = etree.fromstring(response.content)

    try:
        print("Schema Validation:")
        schema_doc = etree.parse("schema.xsd")  
        schema = etree.XMLSchema(schema_doc)  
        schema.assertValid(root)  
        print("XML is valid according to the schema.")
    except etree.DocumentInvalid as e:
        print("XML validation error:", e)
```

In this step, you parse the XSD file using the [`etree.parse()`](https://lxml.de/tutorial.html#the-parse-function) method, then build an XML Schema from the parsed content. Finally, you validate the XML root against that schema using `assertValid()`. If the XML passes validation, you'll see a message like `XML is valid according to the schema`; otherwise, a [`DocumentInvalid`](https://lxml.de/api/lxml.etree.DocumentInvalid-class.html) exception is thrown.

Your output should look like this:

```
 Schema Validation:
    XML is valid according to the schema.
```

Now, let’s read an XML file that uses the `xpath` method to find the elements using their path.

To read the elements using the `xpath()` method, use the following code:

```python
from lxml import etree

import requests

url = "https://brightdata.com/post-sitemap.xml"
response = requests.get(url)

if response.status_code == 200:
   
    root = etree.fromstring(response.content)
    
    print("XPath Support:")
    root = etree.fromstring(response.content)

    namespaces = {"ns": "http://www.sitemaps.org/schemas/sitemap/0.9"}
    for url in root.xpath(".//ns:url/ns:loc", namespaces=namespaces):
        print(url.text.strip())

```

In this snippet, you register the `ns` prefix and link it to the namespace URI `http://www.sitemaps.org/schemas/sitemap/0.9`. The `XPath` expression then uses this prefix to target namespaced elements. Specifically, `.//ns:url/ns:loc` selects all `loc` elements that are children of `url` elements within that namespace.

The output will look like this:

```
XPath Support:

https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

The `find()` and `findall()` methods are faster than `xpath()` since `xpath()` loads all results into memory. Use `find()` unless you need more complex queries.

`lxml` is a powerful library for parsing XML and HTML, supporting advanced features like XPath, schema validation, and XSLT. It's ideal for high-performance or complex tasks but requires separate installation.

If you're working with large or intricate XML data—like financial feeds—`lxml` is a strong choice for efficient querying, validation, and transformation.

## minidom

[`minidom`](https://docs.python.org/3/library/xml.dom.minidom.html) is a simple and lightweight XML parsing library included in Python’s standard library. While not as feature-rich or efficient as `lxml`, it provides an easy way to parse and manipulate XML data.

You can use various DOM methods to access elements. For instance, the [`getElementsByTagName()` method](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagName) allows you to retrieve elements by their tag name.

Here’s an example of using the `minidom` library to parse an XML file and fetch elements by their tag names:

```python
import requests
import xml.dom.minidom

url = "https://brightdata.com/post-sitemap.xml"

response = requests.get(url)
if response.status_code == 200:
    dom = xml.dom.minidom.parseString(response.content)
    
    urlset = dom.getElementsByTagName("urlset")[0]
    for url in urlset.getElementsByTagName("url"):
        loc = url.getElementsByTagName("loc")[0].firstChild.nodeValue.strip()
        print(loc)
else:
    print("Failed to retrieve XML file from the URL.")
```

Your output would look like this:

```
https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

`minidom` represents XML data as a DOM tree, making it easy to navigate and manipulate. It's ideal for basic tasks like reading, modifying, or creating simple XML structures.

If your program needs to read settings from an XML file, `minidom` allows easy access to specific elements, such as finding child nodes or attributes. For example, you can quickly retrieve a `font-size` node and use its value in your application.

## SAX Parser

The [SAX parser](https://docs.python.org/3/library/xml.sax.html) is an event-driven XML parser that processes documents sequentially, emitting events like start tags, end tags, and text content. Unlike DOM parsers, SAX doesn’t load the entire document into memory, making it ideal for large XML files where memory efficiency is important.

To use SAX, you define event handlers for specific XML events, such as `startElement` and `endElement`, which you can customize to handle the document’s structure and content.

Here’s an example of using the SAX parser to process an XML file, defining event handlers to extract URL information from a sitemap:

```python
import requests
import xml.sax.handler
from io import BytesIO

class MyContentHandler(xml.sax.handler.ContentHandler):
    def __init__(self):
        self.in_url = False
        self.in_loc = False
        self.url = ""

    def startElement(self, name, attrs):
        if name == "url":
            self.in_url = True
        elif name == "loc" and self.in_url:
            self.in_loc = True

    def characters(self, content):
        if self.in_loc:
            self.url += content

    def endElement(self, name):
        if name == "url":
            print(self.url.strip())
            self.url = ""
            self.in_url = False
        elif name == "loc":
            self.in_loc = False

url = "https://brightdata.com/post-sitemap.xml"

response = requests.get(url)
if response.status_code == 200:

    xml_content = BytesIO(response.content)
    
    content_handler = MyContentHandler()
    parser = xml.sax.make_parser()
    parser.setContentHandler(content_handler)
    parser.parse(xml_content)
else:
    print("Failed to retrieve XML file from the URL.")
```

Your output would look like this:

```
https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

Unlike other parsers that load the entire file into memory, SAX processes files incrementally, saving memory and improving performance. However, it requires more code to handle each data segment and doesn’t allow revisiting parts of the data for later analysis.

SAX is ideal for efficiently scanning large XML files (e.g., log files) to extract specific information (e.g., error messages). However, if your analysis needs to explore relationships between different data segments, SAX may not be the best choice.

## untangle

[untangle](https://untangle.readthedocs.io/en/latest/) is a lightweight Python library that simplifies XML parsing by allowing you to access XML elements and attributes directly as Python objects. Unlike traditional parsers, which require navigating through hierarchical structures, untangle converts XML documents into nested Python dictionaries. XML elements become dictionary keys, with attributes and text content stored as their corresponding values, making data manipulation easy with standard Python structures.

Untangle is not part of the default Python library and needs to be installed using the following `PyPI` command:

```sh
pip install untangle
```

The following example demonstrates how to parse the XML file using the untangle library and access the XML elements:

```python
import untangle
import requests

url = "https://brightdata.com/post-sitemap.xml"

response = requests.get(url)

if response.status_code == 200:
  
    obj = untangle.parse(response.text)
    
    for url in obj.urlset.url:
        print(url.loc.cdata.strip())
else:
    print("Failed to retrieve XML file from the URL.")
```

Your output will look like this:

```
https://brightdata.com/case-studies/powerdrop-case-study
https://brightdata.com/case-studies/addressing-brand-protection-from-every-angle
https://brightdata.com/case-studies/taking-control-of-the-digital-shelf-with-public-online-data
https://brightdata.com/case-studies/the-seo-transformation
https://brightdata.com/case-studies/data-driven-automated-e-commerce-tools
https://brightdata.com/case-studies/highly-targeted-influencer-marketing
https://brightdata.com/case-studies/data-driven-products-for-smarter-shopping-solutions
https://brightdata.com/case-studies/workplace-diversity-facilitated-by-online-data
https://brightdata.com/case-studies/alternative-travel-solutions-enabled-by-online-data-railofy
https://brightdata.com/case-studies/data-intensive-analytical-solutions
https://brightdata.com/case-studies/canopy-advantage-solutions
https://brightdata.com/case-studies/seamless-digital-automations
```

Untangle simplifies XML parsing in Python by converting XML data into easy-to-use Python objects, eliminating the need for complex navigation. However, it requires separate installation as it’s not part of the core Python package.

Use untangle when you need to quickly convert well-formed XML into Python objects for processing. For example, if you’re working with weather data in XML, untangle can help parse the data and create objects for temperature, humidity, and forecast, which can be easily manipulated in your application.

## Conclusion

Python offers versatile libraries to simplify XML parsing. However, when using the requests library to access files online, you may face quota and throttling issues. [Bright Data](https://brightdata.com/) offers reliable proxy solutions to help bypass these limitations. 

If you'd rather skip the scraping and parsing, check out our [dataset marketplace](https://brightdata.com/products/datasets) for free!