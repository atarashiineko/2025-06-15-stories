# AGENTS.md

## Purpose

Provide rules and directions for a Codex-powered LLM agent that diffs and edits text using HTML annotations and crafts high-quality prose. The agent’s primary goal is to author coherent, engaging stories on any subject, producing well-structured, readable prose that aligns with user guidance and preserves narrative consistency.

## Setup

* Install HTML Tidy (tidy) on the host system
* Ensure tidy is in the agent’s PATH
* Always run tidy with options -indent -wrap 100 on all HTML output before submitting pull requests
* If tidy is not installed, install it automatically before processing

## File Organization

* All story HTML files reside in the docs folder of the repository
* docs/index.html serves as the landing page and lists all story files in a simple HTML list with relative links
* New story files are named <story-name>.html and placed under docs/
* log.md and AGENTS.md remain in the repository root

## Project Structure

* Store all story HTML files in the docs directory
* Maintain an index.html file in docs that lists links to each story using relative paths
* When adding a new story, add a list item linking to it from index.html
* Embed HTML comments in each story summarizing the chain of thought and reasoning behind decisions

## Paragraph ID Specification

Each paragraph element receives an ID of the form: para-<random-segment>
Components
• Prefix: para- (5 characters, lowercase letters and hyphen)
• Random Segment: 11 characters chosen from lowercase letters a–z and digits 0–9, for example 6a853tps1r1
Full ID Shape
• Type: string
• Total length: 16 characters
• Pattern: ^para-\[a-z0-9]{11}\$
Uniqueness and Distribution
• Space size: 36^11 ≈ 4.5 × 10^17 possible values
• Generation method: cryptographically secure random generator, uniform distribution
• Collision avoidance: track IDs in a set during processing; on collision, regenerate until unique
Usage Notes
• Assign each <p> element an ID when wrapping source text
• Store generated IDs in memory or a lookup table to ensure no duplicates
• These IDs enable precise diff/edit operations and reference by ID in commands

## HTML Tidy Formatting Rules

* enable indentation of element content
* set indent-spaces to 2
* wrap lines at 100 characters
* wrap attributes to new lines when they exceed line width
* indent attributes under their element start tag
* preserve existing blank lines between block elements
* ensure each block element opens and closes on its own line
* collapse multiple spaces in text nodes to single spaces
* output in UTF-8 encoding
* use ASCII quote marks for attribute values
* add or preserve alt attributes on img elements
* sort attributes alphabetically within each tag
* remove empty id or class attributes
* enclose boolean attributes without values (for example disabled)
* ensure unique id values remain untouched by tidy
* treat the output as HTML5 and avoid XHTML self-closing syntax

## DIRECTIVES

DIRECTIVE 01: Wrap each paragraph or text block in a semantic HTML element with a unique id following the paragraph ID specification
DIRECTIVE 02: When editing, output only the changed HTML blocks with clear diff markers, for example

* * <p id="para-02">Old text</p>  
* * <p id="para-02">New text</p>  

DIRECTIVE 03: Use plain HTML annotations as code; do not output natural-language descriptions of diffs
DIRECTIVE 04: Always run HTML Tidy on the full document and present the reformatted HTML before finalizing the PR
DIRECTIVE 05: Do not rewrite unchanged blocks; reference them by id when context is needed
DIRECTIVE 06: Use direct, imperative language when issuing commands to the LLM agent, for example Replace the content of <h2 id="sec-03"> with ...
DIRECTIVE 07: Do not include explanatory prose outside of HTML-annotated code blocks in diff outputs
DIRECTIVE 08: Validate that all opened HTML tags are properly closed and that id values are unique
DIRECTIVE 09: When instructed to create a new story, generate docs/<story-name>.html and begin with an outline of major sections
DIRECTIVE 10: Always consider the entire story context when making edits; ensure any change maintains coherence with previous events and narrative flow
DIRECTIVE 11: Maintain Detailed Change Log in log.md After completing any task or feature implementation, append an entry to log.md. Format a Markdown level-2 header containing the ISO 8601 timestamp and a brief title of the task. Under that header include Task Overview, Context, Thought Process, Chosen Solution, Implementation, Impact Summary. Always prepend new entries so the most recent appears at the top
DIRECTIVE 12: Adhere to This Specification File All implementers must follow each directive in this agents.md exactly as written. Any deviation requires explicit sign-off from the project lead
DIRECTIVE 13: Never use nested markdown lists
DIRECTIVE 14: For punctuation always use ASCII alternatives when available
DIRECTIVE 15: Always check if HTML Tidy is installed; if it is not installed, install it automatically before processing. Run tidy with options -indent -wrap 100 to format output HTML with 100 characters of line width and ensure nice human-readable indentation
DIRECTIVE 16: Place all story HTML files in docs/ and update docs/index.html with a list item linking to each new story file
DIRECTIVE 17: When generating story HTML, include the agent’s chain of thought and decision rationale as HTML comments within the file so iterations preserve the reasoning history

## Workflow

1. Receive source text or story request
2. If creating a new story, generate docs/<story-name>.html and produce an outline of major sections
3. Update docs/index.html to include a link to the new story file
4. Wrap source prose in HTML with semantic elements and unique ids
5. Perform diff or edit operations on HTML blocks only, referencing paragraph ids
6. Run HTML Tidy to normalize formatting, enforce 100 character line width and human-readable indentation
7. Embed HTML comments containing thought process and decisions at relevant points in the story file
8. Maintain and update full story context; apply changes coherently across the document
9. Append a detailed entry to log.md per the change log specification
10. Submit the final HTML diff or updated markdown as a pull request
