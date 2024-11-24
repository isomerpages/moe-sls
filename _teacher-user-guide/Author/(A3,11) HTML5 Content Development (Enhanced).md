---
title: (A3,11) HTML5 Content Development (Enhanced)
permalink: /teacher-user-guide/author/html5-content-development/
description: ""
third_nav_title: Author
image: /images/FaviconLight.png
variant: markdown
---
<h1 id="html5-content-development">(A3,11) HTML5 Content Development (Enhanced)</h1>
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
<h2>Creating HTML5 Content for Interactive Response</h2>
<hr>
<ol>
    <li>
        <p>Ensure that the <a href="https://github.com/adlnet/xAPIWrapper/blob/master/dist/xapiwrapper.min.js">XAPI Wrapper JavaScript file (xapiwrapper.min.js)</a> is included in your project. This file provides the necessary functions and utilities for communicating with an Experience API (XAPI) endpoint.</p>
        <p>Before using the XAPI Wrapper functions, it's crucial to load the API wrapper script. This script provides methods for interacting with an XAPI-conformant Learning Record Store (LRS).</p>
        <pre><code>&lt;script src="xapiwrapper.min.js"&gt;&lt;/script&gt;
</code></pre>
        <p>The version used is <a href="https://www.npmjs.com/package/xapiwrapper/v/1.11.0?activeTab=versions">v1.11.0</a></p>
    </li>
    <li>
        <p>Place the following script, it will do the initialization (getParameters) on document ready (DOMContentLoaded), which retrieves parameters (endpoint, auth, agent, stateId, activityId) from the URL query string and configures the xAPI Wrapper (ADL.XAPIWrapper) using the retrieved endpoint and auth credentials.</p>
        <pre><code>  &lt;script&gt;
  // Using a namespace to prevent global variable clashes
  const XAPIUtils = {
    parameters: null, // Parameters store
    getParameters: function () {
      if (!this.parameters) { // Ensure fetch once
        var urlParams = new URLSearchParams(window.location.search);
        var endpoint = urlParams.get('endpoint');
        var auth = urlParams.get('auth');
        var agent = JSON.parse(urlParams.get('agent'));
        var stateId = urlParams.get('stateId');
        var activityId = urlParams.get('activityId');
        ADL.XAPIWrapper.changeConfig({"endpoint": endpoint + "/", "auth": `Basic ${auth}`});
        this.parameters = { agent, stateId, activityId };
      }
      return this.parameters;
    }
  };
  // Immediately invoke getParameters on page load
  document.addEventListener("DOMContentLoaded", function() { XAPIUtils.getParameters(); });
  &lt;script&gt;
</code></pre>
    </li>
    <li>
        <p>Currently SLS supports a single method, sendScore(score). Following is the sample to send the state via <code>ADL.XAPIWrapper.sendState</code> method.</p>
        <pre><code>  &lt;script&gt;
  function sendScore(score) {
    try {
      const parameters = XAPIUtils.getParameters(); // Retrieve parameters from store
      const activityid = parameters.activityId;
      const stateId = parameters.stateId;
      const agent = parameters.agent;
      const registration = null;
      const stateValue = { score: score };
      ADL.XAPIWrapper.sendState(activityid, agent, stateId, registration, stateValue);
    } catch (err) { console.error("An error has occurred!", err); }
  }
  &lt;/script&gt;
</code></pre>
    </li>
    <li>
        <p>Download sample package here</p>
    </li>
</ol>
<p><a href="https://prod-files-secure.s3.us-west-2.amazonaws.com/1b3e5107-e791-4f62-bc5f-70c8a0dd6363/81d88f2d-7b58-4e3f-9be4-e1ebddc4ea95/sample-html5-package.zip">sample-html5-package.zip</a></p>
<ol>
    <li>In the Module Editor page, hover over Question in the Component Bar, select <strong>Free-Response</strong> followed by <strong>Interactive Response.</strong></li>
    <li>Upload the HTM5 ZIP file into SLS and select <strong>Upload</strong> to proceed.</li>
</ol>
<h2><strong>Supported Scenarios for Creating Interactive Response</strong></h2>
<ol>
    <li>You may download the HTML5 ZIP files based on the scenarios to create FRQ with Interactive Response.</li>
</ol>
<h2 id="faqs">FAQs</h2>
<hr>
<ol>
<li><p><strong>Why does the HTML5 Media Object close after an Assignment is opened in a new tab?</strong></p>
<p> The following line of code in the HTML5 Media Object may have caused this issue:</p>
<p><em>top.close();</em></p>
<p>//window.opener.top.close();</p>
<p> As a workaround, students can access the assignment via the Assignment URL.</p>
</li>
<li><p><strong>Why does the HTML5 Media Object appear as a file, instead of being loaded in the frame?</strong></p>
<p> This happens when there is no "index.html" file found directly inside the ZIP file, e.g. it is found inside another folder, or it has been renamed as "index.html.html". Please follow steps 1 and 2 as stated in the User Guide above when uploading a HTML5 Zip file in SLS. 
</p>
</li></ol>