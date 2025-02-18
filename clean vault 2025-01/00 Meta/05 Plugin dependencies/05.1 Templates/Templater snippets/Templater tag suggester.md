<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags())) %>
<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", ""))) %>