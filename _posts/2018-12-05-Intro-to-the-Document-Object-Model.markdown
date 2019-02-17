---
layout: post
title: Introduction to the Document Object Model
---

One of the powers of JavaScript is its ability to create websites that can dynamically interact with users. As part of this interaction, JavaScript can change aspects of a webpage in response to user-generated events. To change an HTML element, JavaScript needs access to it, which is what the **Document Object Model (DOM)** was designed to provide. 

When loading a web page, a browser creates a DOM of the page, which is an **application programming interface (API)** that represents an HTML, XHTML, or XML document as a tree structure. We’ll go into APIs in more depth later on, but an API is a set of clearly defined methods used to communicate among different components. 

By defining the logical structure of a document and the methods for accessing and manipulating a document, the DOM acts as an interface between JavaScript and the HTML to allow the creation of dynamic web pages. DOM methods allow programmatic access to the tree structure, which can be used to change the document's structure, style or content.

In the DOM, the contents of an HTML document are organized into a tree structure, called the **DOM tree**. The DOM tree consists of **nodes**, which can represent HTML elements, text, or comments. As a reminder, an HTML element typically consists of an opening tag, e.g., <p>; some content; and a closing tag, e.g., </p>. 

To give a graphical example of a DOM tree, we'll look at the [tree example from w3](https://www.w3.org/TR/DOM-Level-3-Core/introduction.html). In this example, the HTML file is as follows:
```html
<table>
  <tbody> 
    <tr> 
      <td>Shady Grove</td>
      <td>Aeolian</td> 
    </tr> 
    <tr>
      <td>Over the River, Charlie</td>        
      <td>Dorian</td> 
    </tr> 
  </tbody>
</table>
```

The DOM tree representation of the above HTML would look like this:

![DOM tree representation]({{leila-alderman.github.io}}/assets/DOMtable.png)

The topmost node in the DOM tree is always the document object. Therefore, **“document”** is the parent node to every other node. Similar to HTML, a **child node** is one that is nested inside its **parent node**. If a parent node has more than one child node, these children are considered **siblings**.

## Finding Nodes

Now that we’ve gone over the basics of the DOM, let’s explore how we can start interacting with it. In order to create interactivity by changing HTML elements, we first need to be able to find the HTML elements we want to change. You can select the desired nodes in the DOM using two different types of selectors: CSS selectors and relational selectors.

As a refresher, **CSS selector syntax** is the notation used in style sheets to select which elements the given style should be applied to. 

 * Tag: The selector “p” finds elements with the <p></p> HTML tag.
 * Class: The selector “.abc” finds all elements with `class="abc"`.
 * ID: The selector “#xyz” finds the element with `id="xyz"`, which should be unique.

**Relational selectors** offer an alternative method of moving through the DOM tree using the relationships between nodes. These relationships are contained within the nodes as properties. For example, the following properties can be used to move through the DOM tree:

 * `parentNode`
 * `firstChild`
 * `lastChild`
 * `nextSibling`
 * `previousSibling`

## Accessing Nodes with CSS Selectors

Now that we’ve looked at different methods for selecting elements, we’ll learn how to apply these tools to access nodes. As a reminder, the document object is always the topmost node in the DOM tree. Therefore, to access any HTML element, you must always start by accessing the **document** object.

The DOM interface offers properties (i.e., values) and methods (i.e., actions) that are the primary tools for manipulating a web page with JavaScript. The method that we’ll start with is **.querySelector()**, which returns the first value that matches the selector it’s given. This method is typically the most general as it can accept all CSS style selectors, allowing it to select by tag, class, or ID. For example, to return the first element with the “container” class:

```javascript
const firstContainer = document.querySelector(".container");
```


To return all of the elements that match a given selector, use the .querySelectorAll() method. For example, to return all of the div elements in a page:

```javascript
const allDivElements = document.querySelectorAll("div");
```


## Accessing Nodes with CSS-based Methods

JavaScript also offers more specific methods that can be used to select DOM nodes. When using these methods, note that the CSS syntax (i.e., "#" and ".") is not used. 

First, the **.getElementsbyTagName()** method finds all of the elements that have the given HTML tag. For example, to return all of the div elements in a page, you can use the following statement. Note that this statement is equivalent to the statement above.

```javascript
const allDivElements = document.getElementsbyTagName("div");
```


Second, the **.getElementsbyClassName()** method finds all of the elements that have the given class name. For example, to return all of the elements with the class “container”:

```javascript
const allContainers = document.getElementsbyClassName("container"); 
```


Third, the **.getElementbyID()** method returns the single element with the given ID. Note that this method is intended to return only a single element by design because HTML IDs should be uniquely applied to only one element. For example, to return the element with the ID “intro”:

```javascript
const intro = document.getElementbyID("intro");
```


## Accessing Nodes with Relational Selectors

Finally, JavaScript has the ability to access nodes using their relationship to other nodes. To begin with, every node has a **parentNode** property that points the node it is part of. 

Similarly, if a node has other nodes nested inside of it, the **childNodes** property will return all of those child nodes in an array-like object called a **node list**. A node list looks very similar to an array but does not inherit any of the useful JavaScript array methods. A simple way to create an array from a node list is to use the [**Array.from()** method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from). For example, to obtain an array of child nodes for the first <tr> element in the image above:

```javascript
const firstTr = document.querySelector("tr");
let trChildNodes = firstTr.childNodes;
let trChildNodesArray = Array.from(trChildNodes);
```


In addition to the childNodes property, all parent nodes also have the **children** property, which acts similar to childNodes but returns only element nodes, omitting nodes of other types like text or comments. This property also returns a node list, not an array.

To better understand how the other relational selectors work, the following image from [Eloquent JavaScript](http://eloquentjavascript.net/14_dom.html) shows how the different relational properties can be used to access different nodes:

![DOM relationships]({{leila-alderman.github.io}}/assets/DOMrelations.svg)


## Summary

The Document Object Model (DOM) serves as the interface between JavaScript and HTML. Several different methods can be used to find and access DOM nodes from JavaScript: 

 * CSS selectors used with the `.querySelector()` and `.querySelectorAll()` methods,
 * specific CSS-based methods, such as the `.getElementbyID()` method, and
 * relational selectors, such as the `.childNodes` property. 