---
title: "Catalog of OME-NGFF images"
---
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "Catalog",
  "inLanguage": "en-US",
  "name": "OME-NGFF images"
  "publisher": {
    "@type": "Organization",
    "name": "GitHub"
  },
  "copyrightYear": "2022",
  "discussionUrl": "https://github.com/ome/ome-ngff-catalog-template/issues"
}
</script>

<style>
    .page-content .wrapper {
        box-sizing: border-box;
        width: 100%;
        max-width: 100%;
    }
    .dataTables_scrollHeadInner {
        margin: 0 auto;
    }
    .icon {
        width: 24px;
        height: 24px;
    }
    .no_border {
        border: none;
        background: none;
        padding: 0;
    }
    .shake {
        animation: 0.1s linear 0s infinite alternate seesaw;
    }

    @-webkit-keyframes seesaw { from { transform: rotate(-0.05turn) } to { transform: rotate(0.05turn); }  }
    @keyframes seesaw { from { transform: rotate(-0.05turn) } to { transform: rotate(0.05turn); }  }
</style>

<table class="display table" id="table">
    <thead>
<!-- TODO: should be read from data file -->
        <tr>
            <th>OME-NGFF version</th>
            <th>URL</th>
            <th>SizeX</th>
            <th>SizeY</th>
            <th>SizeZ</th>
            <th>SizeC</th>
            <th>SizeT</th>
            <th>Axes</th>
            <th>Wells</th>
            <th>Fields</th>
            <th>Keywords</th>
            <th>License</th>
            <th>DOI</th>
            <th>Date added</th>
        </tr>
    </thead>
    <tbody>

{% for rec in site.data.table %}
{% capture image_name %}{{ rec.["URL"] | split: "/" | last }}{% endcapture %}
{% capture image_id %}{{ image_name | split: "." | first}}{% endcapture %}
        <tr>
            <td>{{ rec["OME-NGFF version"] }}</td>
            <td>
                <a href="{{ rec["URL"] }}">
                    {{ image_name }}
                </a><br>
                <button class="no_border" title="Copy S3 URL to clipboard" onclick="copyTextToClipboard('{{ rec["URL"] }}')">
                    <img class="icon" src="assets/img/copy.png"/>
                </button>
                <a title="View NGFF {% if rec['Wells'] %}Plate{% else %}Image{% endif %} in Vizarr" target="_blank"
                    href="http://hms-dbmi.github.io/vizarr/?source={{ rec["URL"] }}">
                    <img class="icon" src="assets/img/vizarr.png"/></a>
                <a title="Validate NGFF with 'ome-ngff-validator' in new browser tab" target="_blank"
                    href="https://ome.github.io/ome-ngff-validator/?source={{ rec["URL"] }}">
                    <img class="icon" style="opacity: 0.5" src="assets/img/check.png"/></a>
            </td>
            <td>{{ rec.["SizeX"] }}</td>
            <td>{{ rec.["SizeY"] }}</td>
            <td>{{ rec.["SizeZ"] }}</td>
            <td>{{ rec.["SizeC"] }}</td>
            <td>{{ rec.["SizeT"] }}</td>
            <td>{{ rec.["Axes"] }}</td>
            <td>{{ rec.["Wells"] }}</td>
            <td>{{ rec.["Fields"] }}</td>
            <td>{{ rec.["Keywords"] }}</td>
            <td>{{ rec.["License"] }}</td>
{% endfor %}
    </tbody>
</table>

<script>
$(document).ready( function () {
    $('#table').DataTable( {
          "scrollX": true,
          "pageLength": 100
    });
} );


function copyTextToClipboard(text) {
    var textArea = document.createElement("textarea");
    // Place in the top-left corner of screen regardless of scroll position.
    textArea.style.position = 'fixed';

    textArea.value = text;

    document.body.appendChild(textArea);
    textArea.focus();
    textArea.select();

    var successful;
    try {
        successful = document.execCommand('copy');
    } catch (err) {
        console.log('Oops, unable to copy');
    }
    document.body.removeChild(textArea);

    if (successful) {
        // show user that copying happened - update text on element (e.g. button)
        let target = event.target;
        let html = target.innerHTML;
        target.classList.add("shake");
        setTimeout(() => {
            // reset after 1 second
            target.classList.remove("shake");
        }, 1000)
    } else {
        console.log("Copying failed")
    }
}
</script>
