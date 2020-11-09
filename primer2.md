# DCMI Application Profile Primer

[toc]

## Introduction
The Dublin Core Metadata Terms ([DCMIT](#dcmit)) were early pioneers in the era of open vocabulary reuse. Contrasted to vocabularies that are only for a local use, open vocabularies provide terms that can be understood across disparate applications, making sharing among metadata communities possible. DCMI terms are among the most frequently used vocabulary terms in shared metadata, often used in combination with terms from other vocabularies. 

By its nature, the creation of a new metadata (usage) from available open vocabularies creates what can be called an "application profile." (Also commonly called a "metadata application profile.") Application profiles provide the rules that govern the creation and reuse of metadata instances. Their function is both to *explain* the metadata but also to potentially *constrain* the metadata so that it can be deemed to be correct. Application profiles need to be sharable so that data exchange between communities of practice can take place. There is, however, no current standard for the creation of application profiles such that they could be understood outside of the community within which they have been developed. 

The goal of this DCMI project is to create a standard format for a basic set of application profile functions. We recognize that application profiles can be quite complex and the range of use cases is broad. This present effort does not attempt to address all possible use cases, but, in the same spirit of the Dublin Core Metadata Terms, hopes to provide a basic set of functions that can be extended as needed. For more detailed and complex approaches, the validation languages [ShEx](https://shex.io) and [SHACL](https://www.w3.org/TR/shacl/) provide full functionality.

It is our observation that a very common format used to express profiles is in tables with columns for data elements and the primary constraints of data type and cardinality. For that reason we have modeled our application profile description as a tabular file. This does not mean that profile development is limited to tabular data whose underlying data format is comma separated values ([CSV](https://tools.ietf.org/html/rfc4180)); the data elements provided here can be expressed in other formats. (It is also important to note that this model defines only the application profile and is silent on the data format of the instance data that the profile constrains.)

We recognize that a profile can serve a variety of needs: metadata creation support, metadata validation, metadata exchange, metadata selection, and mapping between metadata from different sources. 

In creating this first version of the application profile we assumed that the profile would be describing and constraining instance data that is formated as RDF. For purely practical purposes we felt that this would be the most useful for the community served by DCMI. A subsequent step could include testing this approach for non-RDF data if there is an interest in that.

## Goals
The task group has been guided by these goals:
* Our outcome should support the creation of a simple profile of describing entities and property/value pairs
* That the profile can be rendered in a table or spreadsheet
* That the profile can express basic constraints (such as cardinality and value types)
* It will be designed (initially) to profile RDF instance data
* It can be transformed into a schema or program that can be used to validate instance metadata


## Profile overview

The purpose of a profile is to define and constrain the property/value pairs that occur in instance metadata. These instance data pairs are statements about some thing that the metadata describes, and may be grouped into distinct graphs called shapes. For example, in a metadata schema that describes books and their authors, books and authors are each shapes with their respective descriptive statements; a metadata schema for college courses could have shapes for courses, professors, and students. The profile provides rules governing the creation and use of the metadata, listing properties, their cardinality, valid value types, and giving labels and notes to aid the reader of the profile.

Only the columns for elements that are needed must be included in the profile, and only the propertyID is required. The order of the columns is not significant; they are identified by their column headers. Note that alternate column headers may be used if 

One of the main goals of this work is to allow metadata developers to create very simple application profiles, with the possibility to expand the profile as needed to accommodate more detail. The vocabulary and template are designed such that metadata profile developers can select only those elements and constraints that they need; only the property identifier is required. There is, however, a limit to what can be accomplished with this simple profile scheme and, just as Dublin Core is a basis that does not restrict other vocabulary development it is not intended to limit any metadata to just this schema. 
## Properties
The simplest profile is a list of properties that will be considered valid in your metadata. A property must be one that has been defined previously in an RDF or OWL vocabulary, and must have an IRI identifier. Examples of properties are http://purl.org/dc/terms/title and http://xmlns.com/foaf/spec/#term_familyName. Property IRIs are usually shortened using defined prefixes (see [Prefixes](#prefixes), below), such that these would be rendered as dct:title and foaf:familyName.

The properties are the only elements of the profile that are required. In essence, a profile is based on a list of properties that define the metadata that is being targeted by the profile.  A profile that defines only its propertyIDs has an assumed but undefined RDF node as its subject and an assumed but undefined RDF node as its object.

### Property identifier
***Element:*** <code>propertyID</code> 

The property ID must be the IRI of a vocabulary term entry that is defined elsewhere. It is mandatory and not repeatable.


<img src="https://i.imgur.com/IK5fIsx.png" alt="Property"
	width="150" height="250" />


**In table format:**
|propertyID|
|----|
|dct:creator
|dct:title
|dct:publisher
|dct:date


### Property label

***Element:*** <code>propertyLabel</code>

Identifiers are often not human-friendly, so the table includes a column for a label for the property that can be viewed in the table or displayed by applications that use the profile.

<img src="https://i.imgur.com/VhEemTG.png" alt="property labels"
	width="420" height="250" />


This profile consists of a list of properties with human-readable labels that can be shown in displays for data creators and for metadata users.


The property label is a human-facing label for the entity that can be used in documentation and displays. Labels are optional but highly recommended so that displays are human-friendly. If it is desired to have displays in more than one language, the labels can use RDF-style language tags, e.g. "Author"@en

**Table format:**
|propertyID|propertyLabel|
|----|----
|dct:creator|Author
|dct:title|Book title
|dct:publisher|Publisher
|dct:date|Publication date


### Property note

***Element:*** <code>Note</code>

In many cases it would be desirable to include some explanatory information for the users of the profile, such as a definition of the property or any other information or instructions that are needed to help users of the profile understand its usage.


**Table format:**
|propertyID|propertyLabel|note|
|----|----|----|
|dct:creator|Author|Each author is given in a separate statement
|dct:title|Book title|The title and subtitle of the book|
|dct:publisher|Publisher|The name of the publisher or imprint from the title page|
|dct:date|Publication date|Publication date of books is generally a four-digit year

(same table but using HTML)
<table border=2>
<tr border=4><th>propertyID</th><th>propertyLabel</th><th>note</th></tr>
<tr border=2><td>dct:creator</td><td>Author</td><td>Each author is given in a separate statement</td></tr>
<tr><td>dct:title</td><td>Book title</td><td>The title and subtitle of the book</td></tr>
<tr><td>dct:publisher</td><td>Publisher</td><td>The name of the publisher or imprint from the title page</td></tr>
<tr><td>dct:date</td><td>Publication date</td><td>Publication date of books is generally a four-digit year</td></tr>
</table>


### Property cardinality

**Element**: <code>Mandatory</code><br />
**Element**: <code>Repeatable</code>

In many metadata designs some fields are required while others are not, and some fields are repeatable while others are not. This can be included in the simple profile using the columns "Mandatory" and "Repeatable". These are defined as being Boolean values which means that they can take any Boolean values understood by consuming programming languages, such as "0,1", "T(rue),F(alse)", "y(es),"n(o)", etc.  Either or both of the elements can be included in the profile, as needed. In the absence of these cardinality constraints, applications using this profile will need to assume default values of their own choosing. It is recommended to indicate these requirements in the profile to avoid misunderstandings about the nature of the metadata.

<img src="https://i.imgur.com/jfmcHQe.png" alt="cardinality"
	width="220" height="280" />



**Table format:**

|propertyID|propertyLabel|mandatory|repeatable|
|----|----|----|----|
|dct:creator|Author|n|y|
|dct:date|Publication date|y|n|
|dct:extent|Pages|n|n


### Property value types

In the metadata instance data, each property has a value. These values can be undeclared and therefore not subject to validation, or they can be defined in the profile, which then makes possible checking of the metadata instances for validity. 

For RDF data there are two inter-related value types: the type of the RDF object node (IRI, blank node, or literal) and the more detailed type of the literal value. Both of these are optional elements of the profile and should they not be included applications processing the instance data may assume defaults.


<img src="https://i.imgur.com/8sbO3Fp.png" alt="Datatypes"
	width="230" height="280" />

**Element:** <code>valueNodetype</code>

As stated above, the value node type is the type of RDF node that is in the object position of the RDF triple. It can be IRI, blank node or literal. ([RDF Concepts](https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/#section-rdf-graph))

**Table format:**
|propertyID|propertyLabel|valueNodeType
|----|----|----|
|dct:creator|Author|IRI|
|dct:date|Publication date|literal|
|dct:extent|Pages|literal|


**Element:** <code>valueDatatype</code>

The value datatypes are literal datatypes, as defined in the RDF Concepts specification. ([RDF Concepts - Datatypes](https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/#section-Datatypes)) The majority of these datatypes are defined in the XML schema datatypes specification ([XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes](http://www.w3.org/TR/xmlschema11-2/)). The list of datatypes there called "primitive" cover many of the most common metadata datatypes, including:
 string · boolean · decimal · float · double · duration · dateTime · time · date · gYearMonth · gYear · gMonthDay · gDay · gMonth · anyURI
 
These datatypes are valid for RDF nodes that are literals. They must be preceded by "xsd:" which defines them as members of the XML schema vocabulary with the full IRI of http://www.w3.org/2001/XMLSchema.

If no data type is provided it may be treated as plain text by applications that process the data.


**Table format:**
|propertyID|propertyLabel|valueNodeType|valueDatatype
|----|----|----|----|
|dct:creator|Author|IRI||
|dct:date|Publication date|literal|xsd:date|
|dct:extent|Pages|literal|xsd:decimal|


## Shapes

Up to this point we have described an application profile that is a single list of properties. By definition, a single list of metadata properties describes a single entity or thing. In practice, metadata often describes multiple things with relationships between them. A common example is bibliographic metadata which may separately describe books and authors, with the relationships between them. Another example is that of products, customers and invoices. Yet another defines the entities in a learning environment. These entities are often expressed as the primary elements of a data diagram: 

![](https://i.imgur.com/CYftbqf.jpg)

In the development of validation concepts for RDF data the emphasis is on the set of statements that specify how an entity is described. These statements combine into groups called shapes, where a shape defines the structure that applications can expect to find in a view over a piece of data. 

### Shape identifier & shape label

***Element:*** <code>shapeID</code><br />
***Element:*** <code>shapeLabel</code>

In RDF instance data, each shape is anchored with a single subject node that is an IRI or blank node. In the profile, any value unique to the profile can identify a shape. For readability and to aid in creating useful displays for metadata developers and users, each shape may have an eye-readable label.


<img src="https://i.imgur.com/PebYXex.png" alt="property labels"
	width="300" height="250" />


Because there is often more than one property for a shape, and because there must be a template row for each property, repeating the shape identifier and label in the profile is optional. It is assumed that all property rows following a row that includes a shape identifier are properties within that shape.

Using the diagram above, we can code one of the shapes in our profile template:

**Table format:**
|shapeID|shapeLabel|propertyID|propertyLabel
|----|----|----|----|
|tutors|Tutor|foaf:mailbox|Email|
|||foaf:accountName|UserName|
|||sdo:accessCode|Password|
|||foaf:givenName|Firstname|
|||foaf:familyName|LastName|
|||sdo:gender|Gender|


Note that this table is equivalent to the one above although it repeats the shape ID and label on each row:

|shapeID|shapeLabel|propertyID|propertyLabel
|----|----|----|----|
|tutors|Tutor|foaf:mailbox|Email|
|tutors|Tutor|foaf:accountName|UserName|
|tutors|Tutor|sdo:accessCode|Password|
|tutors|Tutor|foaf:givenName|Firstname|
|tutors|Tutor|foaf:familyName|LastName|
|tutors|Tutor|sdo:gender|Gender|

### Value shape

***Element:*** <code>valueShape</code>

The value of a property can be a shape. In the example above with tutors, students and courses, the course shape has a property that has the tutor shape as a value.

|shapeID|shapeLabel|propertyID|propertyLabel|valueShape
|----|----|----|----|----|
|courses|Course|dct:title|Course name||
|||dct:description|Course description||
|||sdo:instructor|Tutor|tutors|
|tutors|Tutor|foaf:mailbox|Email||
|||foaf:accountName|UserName||

The string in the <code>valueShape</code> column must match exactly and uniquely the content of a shapeID. A row with a valueShape may also include cardinality constraints which define the requirements of the relationship between the "calling" and the "called" shapes.

|shapeID|shapeLabel|propertyID|propertyLabel|valueShape|mandatory|repeatable
|----|----|----|----|----|----|----|
|courses|Course|dct:title|Course name||y|n|
|||dct:description|Course description||y|n|
|||sdo:instructor|Tutor|tutors|y|y|
|tutors|Tutor|foaf:mailbox|Email||y|n|
|||foaf:accountName|UserName||y|n

## Value constraints (deferred)

<code>valueConstraint</code><br />
<code>valueConstraintType</code>

*This feature is still under discussion in the working group and will be finalized in a future version.*

Beyond defining the datatype of the value many metadata applications have additional rules relating to the value. These can take many forms so it may not be possible to give a complete set, but some common examples are:
* upper and lower ranges on numerical values, such as dates, prices or ages
* year less than 2021 (<2021); age greater than 2 but less than 15 (>2 <15); price less than $200 (LT $200)
* finite lists to be used as "pick lists": 
    * "red" "blue" "green"
    * "DepartmentA" "DepartmentB"
* URI stems for vocabularies or locations of valid terms
    * https://schema.org/
    * http://vocab.getty.edu/aat/
* regular expressions or other code snippets to be applied to the value

![](https://i.imgur.com/lnetOFj.png)

# Appendices

## Explainer and constrainer

The application profile template can be thought of as having three functions: structure, explainer, and constrainer. Viewed as a whole, these functions look like:

![](https://i.imgur.com/Uv9zh7z.png)
![](https://i.imgur.com/A3WR7p4.png)

When developing a profile one can think: "What will explain my metadata to others?" and "What constraints do I put on my metadata?"

## Tables and the CSV format

The tabular application profile format will normally be viewed as a table or spreadsheet. For use in programs, the table is assumed to be stored as comma-delimited values, or CSV.  Many programming languages have functions to accept CSV as an input format. Here is an example of a small profile in both table and CSV formats:

**Table format**

|shapeID|shapeLabel|propertyID|propertyLabel|valueShape
|----|----|----|----|----|
|courses|Course|dct:title|Course name||
|||dct:description|Course description||
|||sdo:instructor|Tutor|tutors|
|tutors|Tutor|foaf:mailbox|Email||
|||foaf:accountName|UserName||

**CSV format**

shapeID,shapeLabel,propertyID,propertyLabel,valueShape
courses,Course,dct:title,Course name,
,,dct:description,Course description,
,,sdo:instructor,Tutor,tutors
tutors,Tutor,foaf:mailbox,Email,
,,foaf:accountName,UserName,

Note that CSV is not the only possible format; tables can often be saved in other tabular formats such as tab-delimited values. The DCMI Application Profile tabular format is designed to be compatible with the CSV standard (https://tools.ietf.org/html/rfc4180) but is not limited to that. 

## Unresolved issues

### Multiple values in a cell

There is often a need to include multiple values in a cell. An example is the listing of possible values as a "pick list":

"red" "blue" "green"
"title@en" "título@es" "titre@fr"

Another example we have encountered is when a property value can be either an IRI or a literal string. 

Both of these imply an "OR" between the values. 

### Namespace declarations

At the very least, property identifiers and value types will be IRI-identified names. To avoid overly long and hard to read strings, these are shortened using a stated prefix:

@prefix dc: <http://purl.org/dc/elements/1.1/> .
@prefix sdo: <https://schema.org/> .
@prefix foaf: <http://xmlns.com/foaf/> .

|shapeID|shapeLabel|propertyID|propertyLabel
|----|----|----|----|
|tutors|Tutor|foaf:mailbox|Email|
|||foaf:accountName|UserName|
|||sdo:accessCode|Password|
|||foaf:givenName|Firstname|
|||foaf:familyName|LastName|
|||sdo:gender|Gender|

We could not find a place for the namespace declarations in the profile tabular format itself. We now assume that namespace declarations, and perhaps some administrative information, would be located in a separate file, perhaps following the recommendations of the W3C CSV on the Web working group. (https://www.w3.org/2013/csvw/wiki/Main_Page)

### Use of quotes in cells

Some implementations of tables or of CSV make use of double quotes to indicate that the value is a literal string. Whether or not quotes will be used may depend on the expectations of the applications that will ingest and use the profile. So far the DCMI application profile standard is silent on the use of quotes. However, this requirement possibly interacts with the need for multiple values, and will be re-visited with that discussion.

### Open/closed

In RDF, all graphs are open, meaning that they can be extended with new arcs and nodes representing new information. The purpose of many profiles will be to define a specific metadata set that is complete and excludes anything not included in the profile description. This needs to be defined in the profile. There are two questions:
1. is where to define this - as a characteristic of the entire profile, or on individual shapes?
2. should there be a default?





