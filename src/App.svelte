<script lang="ts">
import { marked, Renderer } from 'marked';
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css';

// markedコードブロックの背景色対応
// コードブロックにhljsクラスがつかない
// https://github.com/markedjs/marked/pull/418#issuecomment-57291402
const renderer = new Renderer();
renderer.code = function(code, lang) {
    const language = hljs.getLanguage(lang) ? lang : 'plaintext';
    const highlited = hljs.highlight(code, { language, theme: 'onedark' }).value;
    return `<pre><code class='hljs ${lang}'>${highlited}</code></pre>`
}

let markdownText = `# My markdown note

\`\`\`javascript
function main() {
    console.log("Hello, world"!);
}

main();
\`\`\`
`;
</script>

<main class='main'>
    <h2>Markdown Editor</h2>

    <div class='editor'>
        <textarea id='markdown-edit' class='textarea' bind:value={markdownText}></textarea>
        <div class='markdown'>
            {@html marked(markdownText, { renderer, gfm: true, mangle: false, headerIds: false })}
        </div>
    </div>
</main>

<style>
.main {
    margin: 0 auto;
    max-width: 1280px;
    height: 100vh;
}

.editor {
    margin-top: 32px;
}

.textarea {
    width: 80%;
    height: 40%;
    min-height: 240px;
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
}

.markdown {
    width: 80%;
    margin: 32px auto 0;
    text-align: left;
}

@media screen and (min-width: 1280px) {
    .editor {
        display: flex;
        height: 80%;
        gap: 32px;
    }

    .textarea {
        width: 100%;
        height: 100%;
    }

    .markdown {
        width: 100%;
        margin: 0 auto;
    }
}
</style>
