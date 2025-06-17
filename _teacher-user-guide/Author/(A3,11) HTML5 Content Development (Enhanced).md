---
title: (A3,11) HTML5 Content Development (Enhanced)
permalink: /teacher-user-guide/author/html5-content-development/
description: ""
third_nav_title: Author
image: /images/FaviconLight.png
variant: markdown
---
<h1 id="html5-content-development">(A3,11) HTML5 Content Development (Enhanced)</h1>
<hr>
<h2 id="considerations-when-creating-html5-content">Considerations when creating HTML5 Content</h2>
<hr>
<p>When creating HTML5 interactive content for SLS, the maximum file size limit is 500 MB. </p>
<ol>
<li>Acceptable languages: Client-side only technologies: HTML5, CSS, and JavaScript. No server-side code or external APIs are permitted.</li>
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
<li>Self-contained HTML5 packages should contain the necessary WebGL libraries (if required) in their root folder. Note that if WebGL is used, all .js and shader assets must be bundled locally.</li>
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
	<p> Note: Your ZIP folder must contain index.html in the root - not nested and must be named index.html. Otherwise, SLS will not recognise the folder.
</p><p><img alt="HTML5 Content Development" style="width: 100%;" src="/images/2Teacher/AU-AddHTML1.png"></p>
</li>
<li><p>Select all the files within the folder and zip/compress them.</p>
</li>
<li>In SLS, select <strong>File from Device</strong> under Text/Media on the Component Bar. Alternatively, click the <strong>Add Media</strong> icon <img style="width:1.5rem; display: inline;" src="/images/Icons/PaperClip.svg"> and <strong>Upload File</strong> in the Rich Text Editor. </li>
<li><p>Upload the ZIP file by either dragging and dropping it into the box, or clicking the box to select a file through the file manager. A sample file is provided <a target="_blank" href="https://go.gov.sg/qtisample">here</a>.
</p></li>
<img alt="HTML5 Content Development" style="width: 80%;" src="/images/2Teacher/AU-AddHTML2.png">
<li>Click <strong>Upload</strong> when done.</li>
<li><p>If the file is valid, it will be uploaded and scanned for viruses.</p>
<p><u>Note</u>: The virus scan will take longer if the file size is large.</p>
</li>
<li>When the file is successfully uploaded, the Media Object will be added to the Module.
</li>
</ol>
<img alt="HTML5 Content Development" style="width: 80%;" src="/images/2Teacher/AU-AddHTML3.png">
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
  &lt;/script&gt;
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
        <p>Download sample package <a target="_blank" href="https://go.gov.sg/html1">here</a></p>
    </li>
    <li>In the Module Editor page, hover over Question in the Component Bar, select <strong>Free-Response</strong> followed by <strong>Interactive Response.</strong></li>
    <li>Upload the HTM5 ZIP file into SLS and select <strong>Upload</strong> to proceed.</li>
<h2>Supported Scenarios for Creating Interactive Response</h2></ol>
<p>Teachers can now set Interactive Response Questions (IRQs) to automatically return scores and teacher feedback.</p>
<p>IRQs will be able to return final score-marks and teacher feedback after March 2025 Update.</p> 
<img alt="HTML5 Return Score and Feedback" style="width: 80%;" src="/images/2Teacher/Au_HTML5returnscore.png">
<p>To do this, upload an xAPI-compliant HTML5 file to the Free-Response Question – Interactive Response Assistant and set up maximum marks possible and optionally the rubrics if desired.</p>
<img alt="Interactive Response Assistant" style="width: 70%;" src="/images/2Teacher/Au_IRQAssistant.png">
<p>You may download the HTML5 ZIP files based on the scenarios to create FRQ with Interactive Response.</p>
<table class="simple-table">
  <tbody>
    <tr>
      <td><strong>Scenario</strong></td>
      <td><strong>Use Case</strong></td>
      <td><strong>Sample Code</strong></td>
    </tr>
    <tr>
      <td>Send score only</td>
      <td>Involves only the score</td>
      <td><a target="_blank" href="https://go.gov.sg/html2">send-score.zip</a></td>
    </tr>
    <tr>
      <td>Send score, feedback and criteria score and feedback</td>
      <td>Involves score, feedback, criteria score and feedback</td>
      <td><a target="_blank" href="https://go.gov.sg/01html5ufin">01 html5-dynamic-input.zip</a></td>
    </tr>
    <tr>
      <td>Send score via Radio and Checkbox</td>
      <td>Involves score</td>
      <td><a target="_blank" href="https://go.gov.sg/02html5ufin">02 html5-save-input-only.zip</a></td>
    </tr>
		    <tr>
      <td>Send score, feedback and criteria score and feedback</td>
      <td>Involves rubrics and “Show and use rubric marks“ is not selected</td>
      <td><a target="_blank" href="https://go.gov.sg/03html5ufin">03 html5-dynamic-input-score-is-text-field.zip</a></td>
    </tr>
		    <tr>
      <td>Send score and feedback only working on a sample 🌡thermometer interactive</td>
      <td>Involves score, feedback, implemented on an interactive of thermometer</td>
      <td><a target="_blank" href="https://go.gov.sg/03html5ufinthermo">thermometer_html5_dynamic.zip</a></td>
    </tr>
  </tbody>
</table>
<h2 id="faqs">FAQs</h2>
<hr>
<ol>
<li><p><strong>Why does the HTML5 Media Object close after an Assignment is opened in a new tab?</strong></p>
The following line of code in the HTML5 Media Object may have caused this issue:
<ul>
<li><em>top.close();</em></li>
<li>//window.opener.top.close();</li></ul>
As a workaround, students can access the assignment via the Assignment URL.
</li><li><p><strong>Why does the HTML5 Media Object appear as a file, instead of being loaded in the frame?</strong></p>
This happens when there is no "index.html" file found directly inside the ZIP file, e.g. it is found inside another folder, or it has been renamed as "index.html.html". Please follow steps 1 and 2 as stated in the User Guide above when uploading a HTML5 Zip file in SLS. You may want to use this tool: <a>https://sg.iwant2study.org/ospsg/index.php/1253</a> to automatically correct the ZIP file into a SLS compatible format.
<p></p>
</li><li><p><strong>How to create a HTML5 media object?</strong></p>
<ol style="list-style-type:lower-alpha;">
	<li>Create a <strong>fully interactive, realistic-looking simulation</strong> of [plant growth] using only <strong>HTML, CSS, and JavaScript</strong>, all within a <strong> single self-contained HTML file</strong>. This file must be immediately usable in LMS platforms like the <strong>Student Learning Space (SLS)</strong> without requiring any external resources or internet access.</li>
	<li><strong>Hide the page title</strong>, margins, and unnecessary UI to maximize vertical space.</li>
	<li><strong>To optimise the file for SLS iframe view:</strong> Should fit width = 100% and height = 450 px.</li>
	<li><strong>Fully responsive</strong>&nbsp;design for both desktop and mobile screens.</li>
</ol>
<p>
</p></li><li><p><strong>How to create HTML5 Media Object with xAPI with score and teacher feedback compliance?</strong></p>
<ol style="list-style-type:lower-alpha;">
<li>Use a free AI-powered IDE like <a target="_blank" href="http://Trae.ai">Trae.ai</a> or <strong>Visual Code Studio with Cline bot Extension</strong></li>
<li>Open a folder with a working xAPI example, such as the unzipped <strong>03-html5-dynamic-input-score-is-text-field</strong> folder.</li>
<li>Include score-marks and teacher feedback after each action. For example:
	<ul>
		<li><strong>t = 0:</strong> Q1 asked, student answered "[response]" – marked ✅ <strong>correct</strong>.</li>
		<li><strong>t = 1:</strong> Q2 asked, student answered "[response]" – marked ❌<strong>incorrect</strong>.</li>
	</ul>
</li><li>Once the interactive behaves as expected, ask the AI to <strong>hide debugging panels if desired</strong>.</li>
	<li>The project should now be ready to export as an <strong>xAPI-compliant zip file</strong> compatible with <strong>SLS</strong>.</li>
			</ol></li></ol>