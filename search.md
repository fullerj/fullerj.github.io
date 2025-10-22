---
layout: page
title: Search
permalink: /search/
description: >
  Find posts, publications, and talks by keyword across EmpiricalDefense.
---

<div class="search-pane">
  <label class="search-pane__label" for="search-input">Search EmpiricalDefense</label>
  <input id="search-input" type="search" class="search-pane__input" placeholder="Type to search…" autocomplete="off" spellcheck="false">
  <ul id="search-results" class="search-pane__results"></ul>
</div>

<script src="{{ '/assets/js/simple-jekyll-search.min.js' | relative_url }}"></script>
<script>
  document.addEventListener('DOMContentLoaded', function() {
    if (!window.SimpleJekyllSearch) {
      return;
  }

  SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('search-results'),
      json: '{{ "/search.json" | relative_url }}',
      searchResultTemplate: '<li class="search-result"><a class="search-result__link" href="{url}">{title}</a><span class="search-result__meta">{date}</span><p class="search-result__excerpt">{excerpt}</p></li>',
      noResultsText: '<li class="search-result search-result--empty">No results found.</li>',
    limit: 30
  });

  var inputEl = document.getElementById('search-input');
  if (inputEl) {
    inputEl.addEventListener('keydown', function(event) {
      if (event.key === 'Enter') {
        event.preventDefault();
      }
    });
  }
});
</script>
