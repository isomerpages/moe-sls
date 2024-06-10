---
title: (A3,11) HTML5 Content Development
permalink: /teacher-user-guide/author/html5-content-development/
description: ""
third_nav_title: Author
image: /images/FaviconLight.png
variant: markdown
---
<h1 id="html5-content-development">(A3,11) HTML5 Content Development</h1>
<h2 id="considerations-when-creating-html5-content">Considerations when creating HTML5 Content</h2>
<hr>
<p>When creating HTML5 interactive content for SLS, the maximum file size limit is 500 MB. </p>
<ol>
<li>Acceptable languages: HTML5, CSS, and JavaScript (client-side only)</li>
<li>Responsive requirements: 
<ol style="list-style-type: lower-alpha;">
<li>Vector graphics should be used, using SVG or other suitable formats,</li>
<li>The media object should be scaled proportionally in response to changing browser size - for HTML tags, ensure that the dimensions are defined using relative sizes (%, em) instead of absolute sizing (px).</li>
</ol>
</li>
<li>Output:
<ol style="list-style-type: lower-alpha;">
<li>Self-contained HTML5 (does not require an internet connection to work),</li>
	<li>Scales proportionally and works within an &lt;iframe&gt;
 regardless of the dimension of the &lt;iframe&gt;,</li>
<li>Scales proportionately and works within its own browser window regardless of size,</li>
<li>Self-contained HTML5 packages should contain the necessary WebGLibraries (if required) in their root folder.</li>
</ol>
</li>
<li>Others:
<ol style="list-style-type: lower-alpha;">
<li>There should not be any external libraries, including font libraries, when creating the HTML5 interactive content.</li>
</ol>
</li>
</ol>
<h2 id="uploading-a-html5-zip-file-in-sls">Uploading a HTML5 ZIP file in SLS</h2>
<hr>
<ol>
<li><p>Ensure your Media Object folder contains the "index.html" file.</p>
<p><img style="width: 100%;" src="/images/2Teacher/AU-AddHTML1.png"></p>
</li>
<li><p>Select all the files within the folder and zip/compress them.</p>
</li>
<li>In SLS, select <strong>File from Device</strong> under Text/Media on the Component Bar. Alternatively, click the <strong>Add Media</strong> icon <img style="width:1.5rem; display: inline;" src="/images/Icons/PaperClip.svg"> and <strong>Upload File</strong> in the Rich Text Editor. </li>
<li><p>Upload the ZIP file by either dragging and dropping it into the box, or clicking the box to select a file through the file manager. A sample file is provided <a target="_blank" href="https://iwant2study.org/lookangejss/00workshop/2019twa/ejss_model_BalancingChemEqns21_ionic.zip">here</a>
</p></li>
<img style="width: 100%;" src="/images/2Teacher/AU-AddHTML2.png">
<li>Click <strong>Upload</strong> when done.</li>
<li><p>If the file is valid, it will be uploaded and scanned for viruses.</p>
<p><u>Note</u>: The virus scan will take longer if the file size is large.</p>
</li>
<li>When the file is successfully uploaded, the Media Object will be added to the Module.
</li>
</ol>
<img style="width: 100%;" src="/images/2Teacher/AU-AddHTML3.png">

<h2 id="faqs">FAQs</h2>
<hr>
<ol>
<li><p><strong>Why does the HTML5 Media Object close after an Assignment is opened in a new tab?</strong></p>
<p> The following line of code in the HTML5 Media Object may have caused this issue:</p>
<p><em>top.close();</em></p>
<p>//window.opener.top.close();</p>
<p> As a workaround, students can access the assignment via the Assignment URL.</p>
</li>
</ol>