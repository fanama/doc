<div id="file" class="md"><article class="markdown-body entry-content container-lg"><table>
  <thead>
  <tr>
  <th>name</th>
  <th>user-invocable</th>
  <th>description</th>
  <th>allowed-tools</th>
  </tr>
  </thead>
  <tbody>
  <tr>
  <td><div dir="auto">markdown-documentation</div></td>
  <td><div dir="auto">false</div></td>
  <td><div dir="auto">Use when writing technical documentation, READMEs, or project documentation in markdown. Covers structure, conventions, and best practices.</div></td>
  <td><div dir="auto"><table>
  <tbody>
  <tr>
  <td><div dir="auto">Read</div></td>
  <td><div dir="auto">Write</div></td>
  <td><div dir="auto">Edit</div></td>
  <td><div dir="auto">Grep</div></td>
  <td><div dir="auto">Glob</div></td>
  </tr>
  </tbody>
</table>
</div></td>
  </tr>
  </tbody>
</table>

<div class="markdown-heading" dir="auto"><h1 class="heading-element" dir="auto">Markdown Documentation</h1><a id="user-content-markdown-documentation" class="anchor" aria-label="Permalink: Markdown Documentation" href="#markdown-documentation"></a></div>
<p dir="auto">Best practices for writing effective technical documentation in markdown.</p>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">README Structure</h2><a id="user-content-readme-structure" class="anchor" aria-label="Permalink: README Structure" href="#readme-structure"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Minimal README</h3><a id="user-content-minimal-readme" class="anchor" aria-label="Permalink: Minimal README" href="#minimal-readme"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-mh"># <span class="pl-en">Project Name</span></span>

Brief description of what this project does.

<span class="pl-mh">## <span class="pl-en">Installation</span></span>

Instructions to install.

<span class="pl-mh">## <span class="pl-en">Usage</span></span>

Basic usage example.

<span class="pl-mh">## <span class="pl-en">License</span></span>

MIT</pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Comprehensive README</h3><a id="user-content-comprehensive-readme" class="anchor" aria-label="Permalink: Comprehensive README" href="#comprehensive-readme"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-mh"># <span class="pl-en">Project Name</span></span>

<span class="pl-s">![</span>Build Status<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">badge-url</span><span class="pl-s">)</span>
<span class="pl-s">![</span>Version<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">badge-url</span><span class="pl-s">)</span>

One-paragraph description of the project.

<span class="pl-mh">## <span class="pl-en">Features</span></span>

<span class="pl-v">-</span> Feature one
<span class="pl-v">-</span> Feature two
<span class="pl-v">-</span> Feature three

<span class="pl-mh">## <span class="pl-en">Installation</span></span>

<span class="pl-mh">### <span class="pl-en">Prerequisites</span></span>

<span class="pl-v">-</span> Requirement 1
<span class="pl-v">-</span> Requirement 2

<span class="pl-mh">### <span class="pl-en">Steps</span></span>

Instructions...

<span class="pl-mh">## <span class="pl-en">Usage</span></span>

<span class="pl-mh">### <span class="pl-en">Basic Example</span></span>

Code example...

<span class="pl-mh">### <span class="pl-en">Advanced Usage</span></span>

More examples...

<span class="pl-mh">## <span class="pl-en">Configuration</span></span>

Configuration options...

<span class="pl-mh">## <span class="pl-en">API Reference</span></span>

API documentation...

<span class="pl-mh">## <span class="pl-en">Contributing</span></span>

See <span class="pl-s">[</span>CONTRIBUTING.md<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">CONTRIBUTING.md</span><span class="pl-s">)</span>

<span class="pl-mh">## <span class="pl-en">License</span></span>

MIT License - see <span class="pl-s">[</span>LICENSE<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">LICENSE</span><span class="pl-s">)</span></pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Document Organization</h2><a id="user-content-document-organization" class="anchor" aria-label="Permalink: Document Organization" href="#document-organization"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">File Naming</h3><a id="user-content-file-naming" class="anchor" aria-label="Permalink: File Naming" href="#file-naming"></a></div>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>docs/
├── README.md           # Entry point
├── CONTRIBUTING.md     # Contribution guidelines
├── CHANGELOG.md        # Version history
├── CODE_OF_CONDUCT.md  # Community guidelines
├── getting-started.md  # Onboarding guide
├── api/
│   ├── README.md       # API overview
│   └── endpoints.md    # Endpoint reference
└── guides/
    ├── installation.md
    └── configuration.md
</code></pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Linking Between Documents</h3><a id="user-content-linking-between-documents" class="anchor" aria-label="Permalink: Linking Between Documents" href="#linking-between-documents"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre>See the <span class="pl-s">[</span>installation guide<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">./guides/installation.md</span><span class="pl-s">)</span> for details.

For API reference, check <span class="pl-s">[</span>endpoints<span class="pl-s">]</span><span class="pl-s">(</span><span class="pl-corl">./api/endpoints.md#authentication</span><span class="pl-s">)</span>.</pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Writing Style</h2><a id="user-content-writing-style" class="anchor" aria-label="Permalink: Writing Style" href="#writing-style"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Be Concise</h3><a id="user-content-be-concise" class="anchor" aria-label="Permalink: Be Concise" href="#be-concise"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-c"><span class="pl-c">&lt;!--</span> Bad <span class="pl-c">--&gt;</span></span>
In order to install the application, you will need to run the following command.

<span class="pl-c"><span class="pl-c">&lt;!--</span> Good <span class="pl-c">--&gt;</span></span>
Install the application:</pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Use Active Voice</h3><a id="user-content-use-active-voice" class="anchor" aria-label="Permalink: Use Active Voice" href="#use-active-voice"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-c"><span class="pl-c">&lt;!--</span> Bad <span class="pl-c">--&gt;</span></span>
The configuration file should be created in the home directory.

<span class="pl-c"><span class="pl-c">&lt;!--</span> Good <span class="pl-c">--&gt;</span></span>
Create the configuration file in your home directory.</pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Address the Reader</h3><a id="user-content-address-the-reader" class="anchor" aria-label="Permalink: Address the Reader" href="#address-the-reader"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-c"><span class="pl-c">&lt;!--</span> Bad <span class="pl-c">--&gt;</span></span>
Users can configure the timeout setting.

<span class="pl-c"><span class="pl-c">&lt;!--</span> Good <span class="pl-c">--&gt;</span></span>
You can configure the timeout setting.</pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Code Documentation</h2><a id="user-content-code-documentation" class="anchor" aria-label="Permalink: Code Documentation" href="#code-documentation"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Inline Code vs Code Blocks</h3><a id="user-content-inline-code-vs-code-blocks" class="anchor" aria-label="Permalink: Inline Code vs Code Blocks" href="#inline-code-vs-code-blocks"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre>Use <span class="pl-s">`</span><span class="pl-c1">npm install</span><span class="pl-s">`</span> to install dependencies.

For multiple commands, use a code block:

<span class="pl-s">```</span><span class="pl-en">bash</span>
npm install
npm run build
npm start</pre></div>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>
### Command Examples

Show both command and output:

```bash
$ npm --version
10.2.0
</code></pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Configuration Examples</h3><a id="user-content-configuration-examples" class="anchor" aria-label="Permalink: Configuration Examples" href="#configuration-examples"></a></div>
<p dir="auto">Always show complete, valid examples:</p>
<p dir="auto">Create <code>config.json</code>:</p>
<div class="highlight highlight-source-json notranslate position-relative overflow-auto" dir="auto"><pre>{
  <span class="pl-ent">"port"</span>: <span class="pl-c1">3000</span>,
  <span class="pl-ent">"debug"</span>: <span class="pl-c1">true</span>,
  <span class="pl-ent">"database"</span>: {
    <span class="pl-ent">"host"</span>: <span class="pl-s"><span class="pl-pds">"</span>localhost<span class="pl-pds">"</span></span>,
    <span class="pl-ent">"name"</span>: <span class="pl-s"><span class="pl-pds">"</span>myapp<span class="pl-pds">"</span></span>
  }
}</pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Admonitions and Callouts</h2><a id="user-content-admonitions-and-callouts" class="anchor" aria-label="Permalink: Admonitions and Callouts" href="#admonitions-and-callouts"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">GitHub-Style Alerts</h3><a id="user-content-github-style-alerts" class="anchor" aria-label="Permalink: GitHub-Style Alerts" href="#github-style-alerts"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-ent"><span class="pl-ent">&gt; </span><span class="pl-s">[</span>!NOTE<span class="pl-s">]</span></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span>Useful information that users should know.</span>
<span class="pl-ent"></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span><span class="pl-s">[</span>!TIP<span class="pl-s">]</span></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span>Helpful advice for doing things better.</span>
<span class="pl-ent"></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span><span class="pl-s">[</span>!IMPORTANT<span class="pl-s">]</span></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span>Key information users need to know.</span>
<span class="pl-ent"></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span><span class="pl-s">[</span>!WARNING<span class="pl-s">]</span></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span>Urgent info that needs immediate attention.</span>
<span class="pl-ent"></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span><span class="pl-s">[</span>!CAUTION<span class="pl-s">]</span></span>
<span class="pl-ent"><span class="pl-ent">&gt; </span>Advises about risks or negative outcomes.</span></pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Custom Callouts (Emoji-Based)</h3><a id="user-content-custom-callouts-emoji-based" class="anchor" aria-label="Permalink: Custom Callouts (Emoji-Based)" href="#custom-callouts-emoji-based"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre>⚠️ <span class="pl-s">**</span>Warning<span class="pl-s">**</span>: This action cannot be undone.

💡 <span class="pl-s">**</span>Tip<span class="pl-s">**</span>: Use environment variables for sensitive data.

📝 <span class="pl-s">**</span>Note<span class="pl-s">**</span>: This feature requires version 2.0+.</pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">API Documentation</h2><a id="user-content-api-documentation" class="anchor" aria-label="Permalink: API Documentation" href="#api-documentation"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Endpoint Documentation</h3><a id="user-content-endpoint-documentation" class="anchor" aria-label="Permalink: Endpoint Documentation" href="#endpoint-documentation"></a></div>
<div class="markdown-heading" dir="auto"><h4 class="heading-element" dir="auto">Create User</h4><a id="user-content-create-user" class="anchor" aria-label="Permalink: Create User" href="#create-user"></a></div>
<p dir="auto">Creates a new user account.</p>
<p dir="auto"><strong>Request:</strong> <code>POST /api/users</code></p>
<p dir="auto"><strong>Headers:</strong></p>
<table>
<thead>
<tr>
<th>Header</th>
<th>Value</th>
<th align="center">Required</th>
</tr>
</thead>
<tbody>
<tr>
<td>Content-Type</td>
<td>application/json</td>
<td align="center">✅</td>
</tr>
<tr>
<td>Authorization</td>
<td>Bearer {token}</td>
<td align="center">✅</td>
</tr>
</tbody>
</table>
<p dir="auto"><strong>Body:</strong></p>
<div class="highlight highlight-source-json notranslate position-relative overflow-auto" dir="auto"><pre>{
  <span class="pl-ent">"name"</span>: <span class="pl-s"><span class="pl-pds">"</span>John Doe<span class="pl-pds">"</span></span>,
  <span class="pl-ent">"email"</span>: <span class="pl-s"><span class="pl-pds">"</span>john@example.com<span class="pl-pds">"</span></span>
}</pre></div>
<p dir="auto"><strong>Response (201):</strong></p>
<div class="highlight highlight-source-json notranslate position-relative overflow-auto" dir="auto"><pre>{
  <span class="pl-ent">"id"</span>: <span class="pl-s"><span class="pl-pds">"</span>abc123<span class="pl-pds">"</span></span>,
  <span class="pl-ent">"name"</span>: <span class="pl-s"><span class="pl-pds">"</span>John Doe<span class="pl-pds">"</span></span>,
  <span class="pl-ent">"email"</span>: <span class="pl-s"><span class="pl-pds">"</span>john@example.com<span class="pl-pds">"</span></span>
}</pre></div>
<p dir="auto"><strong>Error (400):</strong></p>
<div class="highlight highlight-source-json notranslate position-relative overflow-auto" dir="auto"><pre>{
  <span class="pl-ent">"error"</span>: <span class="pl-s"><span class="pl-pds">"</span>Invalid email format<span class="pl-pds">"</span></span>
}</pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Function Documentation</h3><a id="user-content-function-documentation" class="anchor" aria-label="Permalink: Function Documentation" href="#function-documentation"></a></div>
<div class="markdown-heading" dir="auto"><h4 class="heading-element" dir="auto"><code>parseConfig(path, options?)</code></h4><a id="user-content-parseconfigpath-options" class="anchor" aria-label="Permalink: parseConfig(path, options?)" href="#parseconfigpath-options"></a></div>
<p dir="auto">Parses a configuration file.</p>
<p dir="auto"><strong>Parameters:</strong></p>
<table>
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>path</td>
<td>string</td>
<td>—</td>
<td>Path to config file</td>
</tr>
<tr>
<td>options.strict</td>
<td>boolean</td>
<td>false</td>
<td>Throw on unknown keys</td>
</tr>
<tr>
<td>options.env</td>
<td>boolean</td>
<td>true</td>
<td>Expand environment variables</td>
</tr>
</tbody>
</table>
<p dir="auto"><strong>Returns:</strong> <code>Config</code> - Parsed configuration object</p>
<p dir="auto"><strong>Throws:</strong></p>
<ul dir="auto">
<li><code>FileNotFoundError</code> - Config file doesn't exist</li>
<li><code>ParseError</code> - Invalid JSON/YAML syntax</li>
</ul>
<p dir="auto"><strong>Example:</strong></p>
<div class="highlight highlight-source-js notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-k">const</span> <span class="pl-s1">config</span> <span class="pl-c1">=</span> <span class="pl-en">parseConfig</span><span class="pl-kos">(</span><span class="pl-s">'./config.json'</span><span class="pl-kos">,</span> <span class="pl-kos">{</span> <span class="pl-c1">strict</span>: <span class="pl-c1">true</span> <span class="pl-kos">}</span><span class="pl-kos">)</span><span class="pl-kos">;</span></pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Changelogs</h2><a id="user-content-changelogs" class="anchor" aria-label="Permalink: Changelogs" href="#changelogs"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Keep a Changelog Format</h3><a id="user-content-keep-a-changelog-format" class="anchor" aria-label="Permalink: Keep a Changelog Format" href="#keep-a-changelog-format"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-mh"># <span class="pl-en">Changelog</span></span>

All notable changes to this project will be documented in this file.

<span class="pl-mh">## <span class="pl-en"><span class="pl-s">[</span>Unreleased<span class="pl-s">]</span></span></span>

<span class="pl-mh">### <span class="pl-en">Added</span></span>
<span class="pl-v">-</span> New feature X

<span class="pl-mh">### <span class="pl-en">Changed</span></span>
<span class="pl-v">-</span> Updated dependency Y

<span class="pl-mh">## <span class="pl-en"><span class="pl-s">[</span>1.2.0<span class="pl-s">]</span> - 2024-01-15</span></span>

<span class="pl-mh">### <span class="pl-en">Added</span></span>
<span class="pl-v">-</span> Feature A
<span class="pl-v">-</span> Feature B

<span class="pl-mh">### <span class="pl-en">Fixed</span></span>
<span class="pl-v">-</span> Bug in feature C

<span class="pl-mh">### <span class="pl-en">Deprecated</span></span>
<span class="pl-v">-</span> Old API endpoint

<span class="pl-mh">## <span class="pl-en"><span class="pl-s">[</span>1.1.0<span class="pl-s">]</span> - 2024-01-01</span></span>

<span class="pl-mh">### <span class="pl-en">Added</span></span>
<span class="pl-v">-</span> Initial release</pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Diagrams</h2><a id="user-content-diagrams" class="anchor" aria-label="Permalink: Diagrams" href="#diagrams"></a></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">Mermaid (GitHub Supported)</h3><a id="user-content-mermaid-github-supported" class="anchor" aria-label="Permalink: Mermaid (GitHub Supported)" href="#mermaid-github-supported"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-s">```</span><span class="pl-en">mermaid</span><span class="pl-c1"></span>
<span class="pl-c1">graph LR</span>
<span class="pl-c1">    A[Start] --&gt; B{Decision}</span>
<span class="pl-c1">    B --&gt;|Yes| C[Action 1]</span>
<span class="pl-c1">    B --&gt;|No| D[Action 2]</span>
<span class="pl-c1">    C --&gt; E[End]</span>
<span class="pl-c1">    D --&gt; E</span>
<span class="pl-c1"></span><span class="pl-s">```</span></pre></div>
<div class="markdown-heading" dir="auto"><h3 class="heading-element" dir="auto">ASCII Diagrams</h3><a id="user-content-ascii-diagrams" class="anchor" aria-label="Permalink: ASCII Diagrams" href="#ascii-diagrams"></a></div>
<div class="highlight highlight-text-md notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-s">```</span><span class="pl-c1"></span>
<span class="pl-c1">┌─────────┐     ┌─────────┐     ┌─────────┐</span>
<span class="pl-c1">│ Client  │────▶│ Server  │────▶│Database │</span>
<span class="pl-c1">└─────────┘     └─────────┘     └─────────┘</span>
<span class="pl-c1"></span><span class="pl-s">```</span></pre></div>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Best Practices</h2><a id="user-content-best-practices" class="anchor" aria-label="Permalink: Best Practices" href="#best-practices"></a></div>
<ol dir="auto">
<li><strong>Start with why</strong>: Explain what the project does and why it exists</li>
<li><strong>Show, don't tell</strong>: Provide working code examples</li>
<li><strong>Keep it current</strong>: Update docs when code changes</li>
<li><strong>Test examples</strong>: Ensure code samples actually work</li>
<li><strong>Use consistent terminology</strong>: Define terms and use them consistently</li>
<li><strong>Provide context</strong>: Link to prerequisites and related docs</li>
<li><strong>Consider your audience</strong>: Write for your users' skill level</li>
<li><strong>Include troubleshooting</strong>: Document common errors and solutions</li>
</ol>
<div class="markdown-heading" dir="auto"><h2 class="heading-element" dir="auto">Common Documentation Files</h2><a id="user-content-common-documentation-files" class="anchor" aria-label="Permalink: Common Documentation Files" href="#common-documentation-files"></a></div>
<table>
<thead>
<tr>
<th>File</th>
<th>Purpose</th>
</tr>
</thead>
<tbody>
<tr>
<td>README.md</td>
<td>Project overview and quick start</td>
</tr>
<tr>
<td>CONTRIBUTING.md</td>
<td>How to contribute</td>
</tr>
<tr>
<td>CHANGELOG.md</td>
<td>Version history</td>
</tr>
<tr>
<td>LICENSE</td>
<td>Legal terms</td>
</tr>
<tr>
<td>CODE_OF_CONDUCT.md</td>
<td>Community guidelines</td>
</tr>
<tr>
<td>SECURITY.md</td>
<td>Security policy</td>
</tr>
<tr>
<td>SUPPORT.md</td>
<td>How to get help</td>
</tr>
</tbody>
</table>
</article></div>
