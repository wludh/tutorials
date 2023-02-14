# CETEIcean: Gettting Started ...

\[CETEIcean\](https://github.com/TEIC/CETEIcean) is a JavaScirpt library that converts TEI elements to HTML elements prefixed by tei-\\\[element\\\].

## Purpose of this lesson

We'll be using CETEIcean for the revised version of the Mio Cid project. We're also using it in the Liber Consolationis project. This lesson was created as a training aid for the Liber Con project.

## Learning Outcomes

This lesson covers:

\*   loading CETEI.js into the project  
\*   methods for calling CETEI.js  
\*   using an external JS file   
\*   using CETEI.js to insert a specific chapter from a TEI file into an existing HTML document

\*\*Tip\*\*: for testing, use the \_live server\_ extension to VS Code for previewing pages.

For demonstration purposes, this lesson uses the Liber Consolationis TEI XML.

## Loading CETEI.js into a project

You do not need Node.js to use CETEIcean.

CETEI.js is the JavaScript library. You can add this file to your project by downloading CETEI.js from the GitHub latest release at \[https://github.com/TEIC/CETEIcean/releases.](https://github.com/TEIC/CETEIcean/releases) 

To keep your web project organized, you probably will want to place the CETEI.js file in a js folder in the root of your web document directory.

Since CETEI.js converts TEI to HTML, you will want a source TEI XML file on your web server, such as liber.xml. To keep your web project organized, you might want to create a data directory in the root of your web document directory. Place your TEI XML file in the data directory.

\### Exercise: linking an HTML file to CETEI.js

Create an HTML file.

Add the following to the \\\<head> of the HTML to link the HTML to the CETEI.js file located in the js subdirectory of your project:

\`\`\`javascript  
\<script src="js/CETEI.js">\</script>  
\`\`\`

The CETEI.js file is the library of code that makes CETEI work. However, you need to add JS code to your HTML in order for the CETEI JS library to display the contents of your TEI project.

There are several methods for calling CETEI.js. Let's look at a few of those methods.

## Methods for calling CETEI.js

In some examples, you will see the custom JS code embedded into an HTML file, perhaps like the following:

\`\`\`javascript  
\<script type="text/javascript">  
   let c = new CETEI();  
   let behaviors = {  
   "tei": {  
     "lb": \["\<br>"\],  
   "teiHeader": null,  
   }  
 };  
 c.addBehaviors(behaviors)  
 c.getHTML5('data/liber.xml', function(data) {  
   document.getElementsByTagName("body")\[0\].appendChild(data);  
 });  
 \</script>  
\`\`\`

This example is not particularly intuitive, and we'll skip over the behaviors aspect for now. But the example should work if you have linked properly to the CETEI.js and you have included the proper directory path to your XML source file. If you view the file in the browser using the live server extension, you should see something like the following:

!\[\](https://user-images.githubusercontent.com/10522393/218858785-9614b9a8-def1-43ea-986b-6ca8ef75d7fe.png)

That's a bit of a mess to read. You can clean it up by adding a CSS file to your project that will reformat the TEI output. You can grab a basic CSS file for TEI at \[https://raw.githubusercontent.com/TEIC/CETEIcean/master/test/CETEIcean.css](https://raw.githubusercontent.com/TEIC/CETEIcean/master/test/CETEIcean.css) 

Download that CSS file to the CSS subdirectory of your project, and then link to the CSS from your HTML file. If you have linked the CSS correctly, then you should see a nicer formatted version of the above.

Review the code in the JS above. Note that the output of the CETEI.js places the contents on the \\\<body> tag of the HTML. This is fine if you are converting the TEI XML to display at that URL on a website. But you may have other HTML markup within the HTML file already that you want to preserve, i.e., to wrap around the contents you're inserting from the TEI.

Instead, of inserting the TEI contents onto the \\\<body> tag, let's insert the TEI contents into its own div of an HTML file already containing other HTML markup.

Create an HTML file with an \\\<h1> heading. Add a \\\<div> below that heading, so that the body of your HTML file is like the following:

\`\`\`html  
\<h1>TEI attached to div tag\</h1>  
\<div id="TEI">\</div>  
\`\`\`

The naming of the ID as TEI is arbitrary. You can use any ID name you want, though remember it for the next step.

Now, we want to add in our custom JS code that will retrieve the TEI via CETEI.js:

\`\`\`javascript  
\<script>  
 var CETEIcean = new CETEI();  
 CETEIcean.getHTML5("data/liber.xml", function(data) {  
   document.getElementById("TEI").appendChild(data);  
 });  
\</script>  
\`\`\`

Notice how this code is simpler than our earlier example. When starting to learn a new type of coding, reduce the complexity to the simplest steps. Then, build complexity step-by-step by adding one feature at a time.

The above code inserts (or, rather, appends) the TEI content to the div element with the ID of TEI. If that doesn't make sense, stop. Review the code until you fully understand how it is working.

Your full HTML file should look like the following:

\`\`\`html  
\<!DOCTYPE html>  
\<html>  
\<head>  
 \<meta charset="UTF-8">  
 \<meta name="viewport" content="width=device-width, initial-scale=1.0">  
 \<meta http-equiv="X-UA-Compatible" content="ie=edge">  
 \<link rel="stylesheet" href="css/CETEIcean.css">  
 \<script src="js/CETEI.js">\</script>  
\</head>  
\<body>  
\<h1>TEI attached to div tag\</h1>  
\<div id="TEI">\</div>  
\<script>  
 var CETEIcean = new CETEI();  
 CETEIcean.getHTML5("data/liber.xml", function(data) {  
   document.getElementById("TEI").appendChild(data);  
 });  
\</script>  
\</body>  
\</html>  
\`\`\`

In the browser, you should see a page that looks like the following:

!\[\](https://user-images.githubusercontent.com/10522393/218863343-84770319-eb17-4403-a47e-e60395c9b0c7.png)

In the above image, the heading "TEI attached to div tag" comes from the h1 element of the HTML. The rest of the file is converted from the TEI.

\## Using an external JS file

Having your custom JS in an HTML becomes problematic if you need to use that same code in multiple files or if your code becomes complex. Let's move that code to an external JS file and link our HTML file to the external JS file.

In VS Code, create an empty file and copy over the the JS code that is between the script tags in the above example. Do not place the script tags in your new JS file. Save the JS file and give it a name of your choice, e.g., liber.js. The file should be in the JS subdirectory of your web project.

In the head of your HTML file, you should link to the external JS file. Remember that also need to link to the CETEI.js library as well as any CSS file for styling the TEI output. You should have lines like the following in the head of your HTML:

\`\`\`html  
 \<link rel="stylesheet" href="css/CETEIcean.css">  
 \<script src="js/CETEI.js">\</script>  
 \<script src="js/liber.js">\</script>  
\`\`\`

In your HTML, you no longer need the script tags. Your HTML would simply be like the following:

\`\`\`html  
\<h1>XML retrieved from external JS\</h1>  
\<div id="TEI">\</div>  
\`\`\`

The benefit of having an external JS file, e.g., liber.js, is that you can call that file from different HTML files in your project. When you need to make a change to the JS code, then you only have to update it once in your .js file and not in every .html file.

\## Using CETEI.JS to insert a specific chapter from a TEI file into an existing HTML document

What if you only want to insert a specific chapter from a TEI file into an existing HTML document? The JS code becomes more complicated. One method for doing that:

\`\`\`javascript  
var CETEIcean = new CETEI();  
CETEIcean.getHTML5("data/liber.xml", function(data) {

 const chapter = document.getElementById("caput-1");

 for (const n of Array.from(data.getElementsByTagName("tei-div")))    
 {  
   if(n.getAttribute("n") == 1) {  
   document.adoptNode(n);   
   chapter.appendChild(n);  
   }  
 }  
});  
\`\`\`

This is not the optimal method but it does give an overview of how to approach the problem.

The line starting "const chapter = ..." simply holds a div with the ID of caput-1 in the variable chapter. The body of the HTML is simple:

\`\`\`html  
\<h1>Retrieves specified chapter\</h1>  
\<div id="caput-1">\</div>  
\`\`\`

Let's look at the TEI to a sense as to what we're trying to retrieve:

\`\`\`xml  
\<div type="chapter" n="1">  
             \<head>Caput I\</head>  
             \<head>Exemplum in persona Melibei.\</head>  
               \<p n="1.1">Quidam juvenis, Melibeus nomine, vir potens et dives, relinquens uxorem et filiam in domo, quas multum diligebat, clauso ostio domus, ivit spatiatum. Tres vero sui vicini et hostes antiqui hoc videntes, appositis scalis ac per fenestras domus intrantes, uxorem Melibei, Prudentiam nomine, verberaverunt fortiter et, filiae ejus plagis quinque appositis, videlicet in oculis, auribus, ore et naso ac manibus, illamque semivivam relinquentes, abierunt.\</p>  
               \<p n="1.2">Melibeus vero post modum reversus, hoc videns coepit magno planctu flendo comas sibi dilaniare vestesque suas quasi more furiosi dilacerare. Uxor autem jam dicta, ut taceret, coepit illum instanter ammonere. Ille vero semper plus clamabat; at illa distulit aliquantulum recordatade verbo Ovidii, De Remedio Amoris, qui dixit:  
                 \<said who="#prud" aloud="true" direct="true">  
                   \<cit rend="blockquote" type="classical">  
                     \<quote>Quis matrem, nisi mentis inops, in funere nati  
                       Flere vetat? non hoc illa monenda loco est.  
                       Cum dederit lacrimas animumque impleverit aegrum,  
                       Ille dolor verbis emoderandus erit.  
                     \</quote>  
                     \<bibl n="001" resp="#TS">(\<author>Ovid\</author>, \<title>Remedia Amoris\</title>,  
                     \<biblScope unit="line" from="127" to="130">127-130\</biblScope>)\</bibl>  
                   \</cit>  
                 \</said>  
               \</p>  
           \</div>  
\`\`\`

Remember that CETEI.js works on the TEI after the tags have been processed and prepended with tei-. Hence, the JS code looks for the tag named "tei-div". In our XML, there are multiple divs with that name. The "n" attribute provides a unique identifier to the div that we want. The rest of the JS code queries for that attribute value and appends it to our HTML output.
