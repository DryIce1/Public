<%"---"%>
cssclass: research-note
type: 
  - "{{itemType}}"{% for type, creators in creators | groupby("creatorType") -%}{% if loop.first %}
{% endif %}{{type | replace("interviewee", "author") | replace("director", "author") | replace("presenter", "author") | replace("podcaster", "author") | replace("programmer", "author") | replace("cartographer", "author") | replace("inventor", "author") | replace("sponsor", "author")  | replace("performer", "author") | replace("artist", "author")}}: "{%- for creator in creators -%}{%- if creator.name %}{{creator.name}}{%- else %}{{creator.lastName}}, {{creator.firstName}}{%- endif %}{% if not loop.last %}; {% endif %}{% endfor %}"{% if not loop.last %}
{% endif %}{%- endfor %}{% if title %}
title: "{{title}}"{% endif %}{% if publicationTitle %}
publication: "{{publicationTitle}}"{% endif %}{% if date %}
date: {{date | format("YYYY-MM-DD")}}{% endif %}{% if archive %}
archive: "{{archive}}"{% endif %}{% if archiveLocation %}
archive-location: "{{archiveLocation}}"{% endif %}
citekey: {{citekey}}
<%"---"%>


## Note
{% macro calloutHeader(color) -%}
{%- if color == "#ffd400" -%}
Highlight
{%- endif -%}
{%- if color == "#5fb236" -%}
Examples
{%- endif -%}
{%- if color == "#2ea8e5" -%}
Definitions
{%- endif -%}
{%- if color == "#a28ae5" -%}
Meta
{%- endif -%}
{%- endmacro -%}

{% persist "annotations" %}
{% set annotations = annotations | filterby("date", "dateafter", lastImportDate) -%}
{% if annotations.length > 0 %}

{%- for annotation in annotations %}
{% if annotation.color == "#ff6666" %}
## {{annotation.annotatedText}}

{%- elif annotation.color !== "#ff6666" %}
>[!quote{% if annotation.color %}|{{annotation.color}}{% endif %}{{calloutHeader(annotation.color)}}{%- endif -%}
{%- if annotation.imageRelativePath %}
>![[{{annotation.imageRelativePath}}]]{% endif %}
{%- if annotation.color !== "#ff6666" %}
>{{annotation.annotatedText}} [(p. {{annotation.pageLabel}})](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.pageLabel}}&annotation={{annotation.id}}){%- endif %}{% if annotation.hashTags %} {{annotation.hashTags}}{% endif %}
{%- if annotation.comment%}
>**{{annotation.comment}}**{%- endif %}

{%- endfor %}
{% endif %}
{% endpersist %}