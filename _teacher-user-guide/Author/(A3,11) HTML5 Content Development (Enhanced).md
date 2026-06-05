---
title: (A3,11) HTML5 Content Development (Enhanced)
permalink: /teacher-user-guide/author/html5-content-development/
description: ""
third_nav_title: Author
image: /images/FaviconLight.png
variant: tiptap
---
<h1>(A3,11) HTML5 Content Development (Enhanced)</h1>
<hr>
<h2>Considerations when creating HTML5 Content</h2>
<hr>
<p>When creating HTML5 interactive content for SLS, the maximum file size
limit is 500 MB.</p>
<ol data-tight="true" class="tight">
<li>
<p>Acceptable languages: Client-side only technologies: HTML5, CSS, and JavaScript.
No server-side code or external APIs are permitted.</p>
</li>
<li>
<p>Responsive requirements:</p>
<ol data-tight="true" class="tight">
<li>
<p>Vector graphics should be used, using SVG or other suitable formats,</p>
</li>
<li>
<p>The media object should be scaled proportionally in response to changing
browser size - for HTML tags, ensure that the dimensions are defined using
relative sizes (%, em) instead of absolute sizing (px).</p>
</li>
</ol>
</li>
<li>
<p>Output:</p>
<ol data-tight="true" class="tight">
<li>
<p>Self-contained HTML5 (does not require an internet connection to work),</p>
</li>
<li>
<p>Scales proportionally and works within an &lt;iframe&gt; regardless of
the dimension of the &lt;iframe&gt;,</p>
</li>
<li>
<p>Scales proportionately and works within its own browser window regardless
of size,</p>
</li>
<li>
<p>Self-contained HTML5 packages should contain the necessary WebGL libraries
(if required) in their root folder. Note that if WebGL is used, all .js
and shader assets must be bundled locally.</p>
</li>
</ol>
</li>
<li>
<p>Others:</p>
<ol data-tight="true" class="tight">
<li>
<p>There should not be any external libraries, including font libraries,
when creating the HTML5 interactive content.</p>
</li>
</ol>
</li>
</ol>
<h2>Uploading a HTML5 ZIP file in SLS</h2>
<hr>
<ol>
<li>
<p>Ensure your Media Object folder contains the "index.html" file.</p>
<p>Note: Your ZIP folder must contain index.html in the root - not nested
and must be named index.html. Otherwise, SLS will not recognise the folder.</p>
<div class="isomer-image-wrapper">
<img style="width: 100%;" height="auto" width="100%" alt="HTML5 Content Development" src="/images/2Teacher/AU-AddHTML1.png">
</div>
</li>
<li>
<p>Select all the files within the folder and zip/compress them.</p>
</li>
<li>
<p>In SLS, select <strong>File from Device</strong> under Text/Media on the
Component Bar. Alternatively, click the <strong>Add Media</strong> icon</p>
<div class="isomer-image-wrapper">
<img style="width:1.5rem; display: inline;" height="auto" width="100%" src="/images/Icons/PaperClip.svg">
</div>
<p>and <strong>Upload File</strong> in the Rich Text Editor.</p>
</li>
<li>
<p>Upload the ZIP file by either dragging and dropping it into the box, or
clicking the box to select a file through the file manager. A sample file
is provided <a href="https://go.gov.sg/qtisample" rel="noopener noreferrer nofollow" target="_blank">here</a>.</p>
</li>
<li>
<p>Click <strong>Upload</strong> when done.</p>
</li>
<li>
<p>If the file is valid, it will be uploaded and scanned for viruses.</p>
<p><u>Note</u>: The virus scan will take longer if the file size is large.</p>
</li>
<li>
<p>When the file is successfully uploaded, the Media Object will be added
to the Module.</p>
</li>
</ol>
<div class="isomer-image-wrapper">
<img style="width: 80%;" height="auto" width="100%" alt="HTML5 Content Development" src="/images/2Teacher/AU-AddHTML3.png">
</div>
<h2>Creating HTML5 Content for Interactive Response</h2>
<hr>
<ol>
<li>
<p>Ensure that the <a href="https://github.com/adlnet/xAPIWrapper/blob/master/dist/xapiwrapper.min.js" rel="noopener noreferrer nofollow" target="_blank">XAPI Wrapper JavaScript file (xapiwrapper.min.js)</a> is
included in your project. This file provides the necessary functions and
utilities for communicating with an Experience API (XAPI) endpoint.</p>
<p>Before using the XAPI Wrapper functions, it's crucial to load the API
wrapper script. This script provides methods for interacting with an XAPI-conformant
Learning Record Store (LRS).</p><pre><code>&lt;script src="xapiwrapper.min.js"&gt;&lt;/script&gt;</code></pre>
<p>The version used is <a href="https://www.npmjs.com/package/xapiwrapper/v/1.11.0?activeTab=versions" rel="noopener noreferrer nofollow" target="_blank">v1.11.0</a>
</p>
</li>
<li>
<p>Place the following script, it will do the initialization (getParameters)
on document ready (DOMContentLoaded), which retrieves parameters (endpoint,
auth, agent, stateId, activityId) from the URL query string and configures
the xAPI Wrapper (ADL.XAPIWrapper) using the retrieved endpoint and auth
credentials.</p><pre><code>  &lt;script&gt;
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
  &lt;/script&gt;</code></pre>
</li>
<li>
<p>Currently SLS supports a single method, sendScore(score). Following is
the sample to send the state via <code>ADL.XAPIWrapper.sendState</code> method.</p><pre><code>  &lt;script&gt;
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
  &lt;/script&gt;</code></pre>
</li>
<li>
<p>Download sample package <a href="https://go.gov.sg/html1" rel="noopener noreferrer nofollow" target="_blank">here</a>
</p>
</li>
<li>
<p>In the Module Editor page, hover over Question in the Component Bar, select <strong>Free-Response</strong> followed
by <strong>Interactive Response.</strong>
</p>
</li>
<li>
<p>Upload the HTM5 ZIP file into SLS and select <strong>Upload</strong> to
proceed.</p>
</li>
<li>
<p>Supported Scenarios for Creating Interactive Response</p>
</li>
</ol>
<p>Teachers can now set Interactive Response Questions (IRQs) to automatically
return scores and teacher feedback.</p>
<p>IRQs will be able to return final score-marks and teacher feedback after
March 2025 Update.</p>
<div class="isomer-image-wrapper">
<img style="width: 80%;" height="auto" width="100%" alt="HTML5 Return Score and Feedback" src="/images/2Teacher/Au_HTML5returnscore.png">
</div>
<p>To do this, upload an xAPI-compliant HTML5 file to the Free-Response Question
– Interactive Response Assistant and set up maximum marks possible and
optionally the rubrics if desired.</p>
<div class="isomer-image-wrapper">
<img style="width: 70%;" height="auto" width="100%" alt="Interactive Response Assistant" src="/images/2Teacher/Au_IRQAssistant.png">
</div>
<p>You may download the HTML5 ZIP files based on the scenarios to create
FRQ with Interactive Response.</p>
<table style="minWidth: 75px">
<colgroup>
<col>
<col>
<col>
</colgroup>
<tbody>
<tr>
<td rowspan="1" colspan="1">
<p><strong>Scenario</strong>
</p>
</td>
<td rowspan="1" colspan="1">
<p><strong>Use Case</strong>
</p>
</td>
<td rowspan="1" colspan="1">
<p><strong>Sample Code</strong>
</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>More sample files and templates to try out</p>
</td>
<td rowspan="1" colspan="1">
<p>more recent examples</p>
</td>
<td rowspan="1" colspan="1">
<p>directly from the <a href="https://iwant2study.moe.edu.sg/lookangejss/appXapiIntegratorAgent/public/" class="ng-star-inserted" rel="noopener" target="_blank">MOE xAPI Integrator Agent</a>.</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>Send score only</p>
</td>
<td rowspan="1" colspan="1">
<p>Involves only the score</p>
</td>
<td rowspan="1" colspan="1">
<p><a href="https://go.gov.sg/html2" rel="noopener noreferrer nofollow" target="_blank">send-score.zip</a>
</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>Send score, feedback and criteria score and feedback</p>
</td>
<td rowspan="1" colspan="1">
<p>Involves score, feedback, criteria score and feedback</p>
</td>
<td rowspan="1" colspan="1">
<p><a href="https://go.gov.sg/01html5ufin" rel="noopener noreferrer nofollow" target="_blank">01 html5-dynamic-input.zip</a>
</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>Send score via Radio and Checkbox</p>
</td>
<td rowspan="1" colspan="1">
<p>Involves score</p>
</td>
<td rowspan="1" colspan="1">
<p><a href="https://go.gov.sg/02html5ufin" rel="noopener noreferrer nofollow" target="_blank">02 html5-save-input-only.zip</a>
</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>Send score, feedback and criteria score and feedback</p>
</td>
<td rowspan="1" colspan="1">
<p>Involves rubrics and “Show and use rubric marks“ is not selected</p>
</td>
<td rowspan="1" colspan="1">
<p><a href="https://go.gov.sg/03html5ufin" rel="noopener noreferrer nofollow" target="_blank">03 html5-dynamic-input-score-is-text-field.zip</a>
</p>
</td>
</tr>
<tr>
<td rowspan="1" colspan="1">
<p>Send score and feedback only working on a sample 🌡thermometer interactive</p>
</td>
<td rowspan="1" colspan="1">
<p>Involves score, feedback, implemented on an interactive of thermometer</p>
</td>
<td rowspan="1" colspan="1">
<p><a href="https://go.gov.sg/03html5ufinthermo" rel="noopener noreferrer nofollow" target="_blank">thermometer_html5_dynamic.zip</a>
</p>
</td>
</tr>
</tbody>
</table>
<h2>FAQs</h2>
<hr>
<ol>
<li>
<p><strong>Why does the HTML5 Media Object close after an Assignment is opened in a new tab?</strong>
</p>
<p>The following line of code in the HTML5 Media Object may have caused this
issue:</p>
<ul data-tight="true" class="tight">
<li>
<p><em>top.close();</em>
</p>
</li>
<li>
<p>//window.opener.top.close();</p>
</li>
</ul>
<p>As a workaround, students can access the assignment via the Assignment
URL.</p>
</li>
<li>
<p><strong>Why does the HTML5 Media Object appear as a file, instead of being loaded in the frame?</strong>
</p>
<p>This happens when there is no "index.html" file found directly inside
the ZIP file, e.g. it is found inside another folder, or it has been renamed
as "index.html.html". Please follow steps 1 and 2 as stated in the User
Guide above when uploading a HTML5 Zip file in SLS. You can use either
of these tools:</p>
<ol data-tight="true" class="tight">
<li>
<p><a href="https://sg.iwant2study.org/ospsg/index.php/1253" class="ng-star-inserted" rel="noopener" target="_blank">SLS Legacy ZIP Fixer</a> (to
fix poorly structured zips)</p>
</li>
<li>
<p><a href="https://iwant2study.moe.edu.sg/lookangejss/slsZipper/Code_to_ZIP_Converter/" class="ng-star-inserted" rel="noopener" target="_blank">MOE Code to ZIP Converter</a> (to
easily bundle your custom HTML5 code into an SLS-compatible ZIP file)</p>
<p></p>
<p></p>
</li>
</ol>
</li>
<li>
<p><strong>How to create HTML5 Media Object with xAPI with score and teacher feedback compliance?</strong>
</p>
<ol>
<li>
<p>Create a <strong>fully interactive, realistic-looking simulation</strong> of
[plant growth] using only <strong>HTML, CSS, and JavaScript</strong>, all
within a <strong>single self-contained HTML file</strong>. This file must
be immediately usable in LMS platforms like the <strong>Student Learning Space (SLS)</strong> without
requiring any external resources or internet access.</p>
</li>
<li>
<p><strong>Hide the page title</strong>, margins, and unnecessary UI to maximize
vertical space.</p>
</li>
<li>
<p><strong>To optimise the file for SLS iframe view:</strong> Should fit width
= 100% and height = 450 px.</p>
</li>
<li>
<p><strong>Fully responsive</strong>&nbsp;design for both desktop and mobile
screens.</p>
</li>
<li>
<p>Visit the <a href="https://iwant2study.moe.edu.sg/lookangejss/appXapiIntegratorAgent/public/" class="ng-star-inserted" rel="noopener" target="_blank">MOE xAPI Integrator</a> and
upload your zip interactive. The integrator automatically inserts all essential
xAPI starter code, files and file structure</p>
<p>The integrator automatically inserts all essential xAPI starter code,
files and file structure, and launch parameters, ensuring:</p>
<ul data-tight="true" class="tight">
<li>
<p>scoring placeholder ready</p>
</li>
<li>
<p>generic teacher feedback ready</p>
</li>
<li>
<p>SLS compatibility</p>
</li>
</ul>
<p><strong>Layer the new customised score and feedback on top of xAPI working interactive.</strong>
</p>
<p>Use a free AI-powered IDE like Trae.ai or Visual Code Studio with Cline
bot Extension</p>
<p>Prompt the IDE to add specific scoring and feedback based on your interactive.</p>
<p>For example, score increment by 1 when certain actions are performed or
hook up the current marks to the score to send via xAPI.</p>
<p>for example, feedback can be strings of actions that student to support
development and assess of 21st century skills, such as critical, adaptive
and inventive thinking .</p>
<p>t = 0: Q1 asked, student answered "[response]" – marked ✅ correct.</p>
<p>t = 1: Q2 asked, student answered "[response]" – marked ❌incorrect.</p>
<p>Once the interactive behaves as expected, ask the AI to hide debugging
panels if desired.</p>
<p>The project should now be ready to export as an xAPI-compliant zip file
compatible with SLS through the Free-Response Question – Interactive Response
Assistant</p>
</li>
<li>
<p>You can leverage full IDE ecosystems like <a href="http://Trae.ai" rel="noopener noreferrer nofollow" target="_blank">Trae.ai</a> or <a href="https://code.visualstudio.com/" class="ng-star-inserted" rel="noopener" target="_blank">Visual Studio Code (VSC)</a> with
the <a href="https://cline.bot/" class="ng-star-inserted" rel="noopener" target="_blank">Cline Bot Extension,</a> 
<a href="https://www.anthropic.com/product/claude-code" class="ng-star-inserted" rel="noopener" target="_blank">Claude Code</a> <a href="https://platform.openai.com/" class="ng-star-inserted" rel="noopener" target="_blank">OpenAI Codex, </a>you can use <a href="https://antigravity.google/" class="ng-star-inserted" rel="noopener" target="_blank">Google Antigravity</a> to
automatically generate specific JavaScript logic directly within your setup.</p>
</li>
<li>
<p>Open a folder with a working xAPI example, such as the unzipped <strong>03-html5-dynamic-input-score-is-text-field</strong> folder.</p>
</li>
<li>
<p>Prompt the IDE to add new interactivity on top of the existing xAPI file.</p>
</li>
<li>
<p>Include score-marks and teacher feedback after each action. For example:</p>
<ul data-tight="true" class="tight">
<li>
<p><strong>t = 0:</strong> Q1 asked, student answered "[response]" – marked
✅ <strong>correct</strong>.</p>
</li>
<li>
<p><strong>t = 1:</strong> Q2 asked, student answered "[response]" – marked
❌<strong>incorrect</strong>.</p>
</li>
</ul>
</li>
<li>
<p>Once the interactive behaves as expected, ask the AI to <strong>hide debugging panels if desired</strong>.</p>
</li>
<li>
<p>The project should now be ready to export as an <strong>xAPI-compliant zip file</strong> compatible
with <strong>SLS</strong>.</p>
</li>
</ol>
</li>
</ol>
<p></p>