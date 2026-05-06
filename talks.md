---
layout: page
title: Speaking and Briefings
permalink: /talks/
description: >
  Invited talks and executive briefings on adversary behavior, detection engineering, and securing complex systems, delivered across leading conferences, research venues, and operational environments.
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<style>
  #talks-map {
    width: 100%;
    height: 500px;
    margin: 2rem 0;
    border-radius: 0.75rem;
    box-shadow: 0 4px 12px rgba(17, 30, 45, 0.12);
    z-index: 10;
  }
  .leaflet-container {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, sans-serif;
  }
  .leaflet-popup-content {
    font-size: 0.9rem;
  }
</style>

<div id="talks-map"></div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
(function() {
  'use strict';
  
  var talks = {{ site.data.talks | jsonify }};
  var mapInitialized = false;
  
  function initMap() {
    if (mapInitialized || !document.getElementById('talks-map')) {
      return;
    }
    mapInitialized = true;
    
    var map = L.map('talks-map').setView([20, 0], 2);
    
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> contributors',
      maxZoom: 19,
      crossOrigin: true
    }).addTo(map);
    
    var bounds = [];
    var markers = {};
    
    talks.forEach(function(talk) {
      if (!talk.latitude || !talk.longitude) return;
      
      var key = talk.latitude + ',' + talk.longitude;
      var popup = '<strong>' + talk.title + '</strong><br>' +
                  '<small>' + talk.event + '</small><br>' +
                  '<small>' + (talk.date ? new Date(talk.date).toLocaleDateString('en-US', { year: 'numeric', month: 'short' }) : 'TBA') + '</small>';
      
      if (markers[key]) {
        markers[key].popup += '<hr>' + popup;
      } else {
        var marker = L.circleMarker([talk.latitude, talk.longitude], {
          radius: 8,
          fillColor: 'rgb(79, 177, 186)',
          color: 'rgb(25, 55, 71)',
          weight: 2,
          opacity: 0.9,
          fillOpacity: 0.7
        }).addTo(map);
        
        marker.popup = popup;
        marker.bindPopup(popup);
        markers[key] = marker;
        bounds.push([talk.latitude, talk.longitude]);
      }
    });
    
    if (bounds.length > 0) {
      var group = new L.featureGroup(Object.values(markers).map(function(m) { return m; }));
      map.fitBounds(group.getBounds().pad(0.1));
    }
  }
  
  var pushState = document.getElementById('_pushState');
  if (pushState) {
    pushState.addEventListener('hy-push-state-after', initMap);
  }
  
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initMap);
  } else {
    initMap();
  }
})();
</script>

{% assign talks = site.data.talks | sort: "date" | reverse %}

{% if talks and talks.size > 0 %}
<section class="talks-section talks-section--timeline">

  <div class="talks-timeline">
    {% for talk in talks %}
      <article class="talk-card talk-card--timeline">
        <p class="talk-card__meta">
          <span>{{ talk.date | date: "%Y" }}</span>
        </p>
        <h3 class="talk-card__title">{{ talk.title }}</h3>
        {% if talk.subtitle %}
          <p class="talk-card__subtitle">{{ talk.subtitle }}</p>
        {% endif %}
        <p class="talk-card__event">{{ talk.event }}{% if talk.location %} - {{ talk.location }}{% endif %}</p>
        <p class="talk-card__date">{% if talk.date %}{{ talk.date | date: "%B %-d, %Y" }}{% else %}TBA{% endif %}</p>
        {% if talk.description %}
          <p class="talk-card__description">{{ talk.description }}</p>
        {% endif %}
        {% if talk.resources %}
          <p class="talk-card__links">
            {% for resource in talk.resources %}
              <a href="{{ resource.url }}">{{ resource.label }}</a>
            {% endfor %}
          </p>
        {% endif %}
      </article>
    {% endfor %}
  </div>
</section>
{% else %}
No talks have been published yet. Check back soon.
{% endif %}
