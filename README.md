Samenvatting Objectgericht Programeren
===============

Samenvatting cursus Objectgericht Programmeren uit Leuven

# Install

Om dit lokaal te kunnen aanpassen heb je ruby en Jekyll nodig. Op mac/linux ken je eenvoudig [dit](https://github.com/github/pages-gem) gebruiken. Op windows kan je [deze tutorial](http://bradleygrainger.com/2011/09/07/how-to-use-github-pages-on-windows.html) proberen, maar je kan beter rechtstreeks `gem install github-pages` gebruiken in stap 3 ipv `jekyll` (de github-pages gem bevat Jekyll).

# Stappenplan installatie -- windows

## Ruby in orde krijgen
* installer ruby downloaden
* devkit downloaden : https://github.com/oneclick/rubyinstaller/wiki/Development-Kit (stappenplan volgen)
* gem sources -a http://rubygems.org/ -> anders problemen bij gem install
* om warnings weg te krijgen (optioneel): gem sources -r https://rubygems.org/
* gem install rouge
* gem install wdm

## Repository
* download sourcetree
* voeg repository toe in sourcetree -> clone naar lokale map
* pas _config.yml aan ("highlighter: false" -> moet er bij (zonder quotes)) -> dit niet pushen!

## Runnen
- Naar cmd met ruby (da is er bijgekomen nadat ge de rubyinstaller hebt gebruikt), dit commando uitvoeren: "jekyll serve"
- localhost:4000 en het zou normaal moeten lukken

## Problemen
- ask via facebookgroep!
