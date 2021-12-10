---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
---

<h1>Welcome to the Quarkus Coffeeshop!</h1>
<p>This is the repository for several demos based around a Coffeeshop</p>
<ul>
<li><a class="page-link" href="/coffeeshop/">Coffeeshop</a></li>
<li><a class="page-link" href="/homeoffice/">Homeoffice</a></li>
<li><a class="page-link" href="/devops/">DevOps</a></li>
<li><a class="page-link" href="/management/">Hybrid Cloud Management</a></li>
</ul>

<h2>Quarkus Coffeeshop</h2>
<p>The Quarkus Coffeeshop is a collection of seven microservices that use Apache Kafka as their transport.<p>
<p>The microservices are:
<ul>
<li>Counter</li>
<li>Web</li>
<li>Barista</li>
<li>Kitchen</li>
<li>Inventory</li>
</ul>
</p>
<p>Supporting files including a docker-compose file are in the quarkuscoffeeshop-support project</p>
<h2>Home Office</h2>
<p>The Home Office consists of three microservices that receive updates from Coffee Shops over Apache Kafka and display data using Red Hat's Patternfly library for ReactJS.<p>
<p>The microservices are:
<ul>
<li>Ingress</li>
<li>Backend</li>
<li>Frontend</li>
</ul>
