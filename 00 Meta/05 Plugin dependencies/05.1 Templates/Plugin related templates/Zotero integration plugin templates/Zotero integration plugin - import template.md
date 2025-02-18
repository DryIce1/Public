<%"---"%>
aliases: 
citekey: {{citekey}}
date-created: <% tp.file.creation_date("YYYY-MM-DD") %>
publish: true
tags: type/source/content
author: {{authors}}{{directors}}
url: {{url}}
type: 
  - article
status: 
priority: 
AutoNoteMover: disable
<%"---"%>
> [!link] Zotero ‎ [PDF]({{attachments[0].pdfURI}}), [URL]({{url}})

{% for annotation in annotations -%} 
{% if annotation.color == "#ffd400" and annotation.type !== "image"  %}- {{annotation.annotatedText}}{%- endif -%}
{% if annotation.color == "#ff6666" %}- ! {{annotation.annotatedText}}{%- endif -%}
{% if annotation.color == "#5fb236" %}- ✔ {{annotation.annotatedText}}{%- endif -%}
{% if annotation.color == "#2ea8e5" %}- 💊 {{annotation.annotatedText}} [(p{{annotation.page}})](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){%- endif -%}
{% if annotation.color == "#a28ae5" and annotation.type !== "image" %}- Note : {{annotation.annotatedText}} [(p{{annotation.page}})](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){%- endif -%}
{% if annotation.color == "#e56eee" %}
## {{annotation.annotatedText}} [.](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){%- endif -%}
{% if annotation.color == "#f19837" %}
# {{annotation.annotatedText}} [.](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}){%- endif -%}
{% if annotation.color == "#aaaaaa" %}> [!QUOTE] {{annotation.annotatedText}}{% endif -%}
{% if annotation.comment and annotation.type !== "image" %}
    - {{annotation.comment}} [(p{{annotation.page}})](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}) {% endif -%}
{% if annotation.type == "image" and annotation.color !== "#a28ae5" %}
> [!image-description] ![[{{annotation.imageBaseName}}]]
> {{annotation.comment}}
{% endif -%}
{% if annotation.type == "image" and annotation.color == "#a28ae5" %}
> [!image-description] ![[{{annotation.imageBaseName}}]]
> {{annotation.comment}} [(p{{annotation.page}})](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})
{% endif %}
{% endfor -%}
