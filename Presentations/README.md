
## Obsidian Slides
- https://help.obsidian.md/Plugins/Slides

To view slides, you'll need to enable the "Slides" plugin.  
This can be done through the Settings => Core plugins.

![[Screenshot 2023-02-17 at 14.15.25.png]]


```
# Presentations using Slides 

A demo on how to build presentations using Slides. 

--- 

## Formatting You can use regular Markdown formatting, like *emphasised* and **bold** text. 

--- 

## Slides Use `---` to separate slides.
```


#### Limitation
- short pages
- can't navigate up and down like some presentations
- can't jump directly to a page
- no ToC to navigate

#### Good
- damn quick building
- damn smooth
- markdown => easy

#### Export 
<style>
  /* To help export pages line by lines */
  @media print { hr { page-break-after: always; } }
</style>

## Marp

https://github.com/marp-team/marp-cli 
- with Bespoke.js for animations https://github.com/marp-team/marp-cli/blob/main/docs/bespoke-transitions/README.md

### Install
```
npm install -g @marp-team/marp-cli
```

### Usage
```
marp slide.md -o output.html --allow-local-files
```
