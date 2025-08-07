---
toc: true
---

```js
const catalogues = FileAttachment("./data/catalogues.json").json();
```

# IPSN PGS Data Standard Catalogue

Experimental project under development.  
Sources: [code for this page](https://github.com/WHO-Collaboratory/collaboratory-pgs-data-standards-page), [catalogue repository](https://github.com/WHO-Collaboratory/collaboratory-pgs-data-standards).

---

## Filters 

```js
const branches = catalogues.map(d => d.branch);
const branchInput = Inputs.select(branches, {label: "Catalogue version"});
const branch = Generators.input(branchInput);
```
```js
const catalogue = catalogues.filter(d => d.branch == branch)[0].catalogue
```
```js
const searchInput = Inputs.search(
    catalogue, 
    {placeholder: "Search catalogue…", format: (d => "")}
);
const search = Generators.input(searchInput);
```
```js
const name_list = catalogue.map(x => x.name).flat();
const name_selectInput = Inputs.checkbox(
    name_list, 
    {label: "Name", unique: true, sort: true}
);
const name_select = Generators.input(name_selectInput);
```
```js
const scopes_list = catalogue.map(x => x.scopes).flat();
const scopes_selectInput = Inputs.checkbox(
    scopes_list, 
    {label: "Scope", unique: true, sort: true}
);
const scopes_select = Generators.input(scopes_selectInput);
```
```js
const targets_list = catalogue.map(x => x.targets).flat();
const targets_selectInput = Inputs.checkbox(
    targets_list, 
    {label: "Target", unique: true, sort: true}
);
const targets_select = Generators.input(targets_selectInput);
```
```js
const formats_list = catalogue.map(x => x.formats).flat();
const formats_selectInput = Inputs.checkbox(
    formats_list, 
    {label: "Format", unique: true, sort: true}
);
const formats_select = Generators.input(formats_selectInput);
```
```js
const supporting_materials_list = catalogue.map(x => x.supporting_materials).flat();
const supporting_materials_selectInput = Inputs.checkbox(
    supporting_materials_list, 
    {label: "Supporting material", unique: true, sort: true}
);
const supporting_materials_select = Generators.input(supporting_materials_selectInput);
```
```js
const developers_list = catalogue.map(x => x.developers).flat();
const developers_selectInput = Inputs.checkbox(
    developers_list, 
    {label: "Developer", unique: true, sort: true}
);
const developers_select = Generators.input(developers_selectInput);
```
<div class="grid grid-cols-3" style="grid-auto-rows: auto;">
<div class="card grid-colspan-3">${branchInput}</div>
<div class="card">${searchInput}</div>
<div class="card grid-colspan-2">${name_selectInput}</div>
<div class="card">${scopes_selectInput}</div>
<div class="card">${targets_selectInput}</div>
<div class="card">${formats_selectInput}</div>
<div class="card">${supporting_materials_selectInput}</div>
<div class="card grid-colspan-2">${developers_selectInput}</div>
</div>

```js
const filtered_catalogue = search.filter(d => 
    (name_select.length == 0 || name_select.some(x => x == d.name)) &&
    (scopes_select.length == 0 || d.scopes.some(x => scopes_select.includes(x))) &&
    (targets_select.length == 0 || d.targets.some(x => targets_select.includes(x))) &&
    (formats_select.length == 0 || d.formats.some(x => formats_select.includes(x))) &&
    (supporting_materials_select.length == 0 || d.supporting_materials.some(x => supporting_materials_select.includes(x))) &&
    (developers_select.length == 0 || d.developers.some(x => developers_select.includes(x)))
);
```

${filtered_catalogue.length} ${(filtered_catalogue.length > 1 ? "entries" : "entry")}  found.

---

## Cards

<div class="grid grid-cols-2">
    ${filtered_catalogue.map(
        d => html`<div class="card">
            <h1>${d.name}</h1>
            <h4>URLs</h4>
            <p>${d.URLs.map(
                x => htl.html`<a href="${x}">${x}</a><br>`
            )}</p>
            <h4>Scopes</h4>
            <p>${d.scopes.length == 0 ? '-' : d.scopes.join(", ")}</p>
            <h4>Targets</h4>
            <p>${d.targets.length == 0 ? '-' : d.targets.join(", ")}</p>
            <h4>Formats</h4>
            <p>${d.formats.join(", ")}</p>
            <h4>Supporting materials</h4>
            <p>${d.supporting_materials.length == 0 ? '-' : d.supporting_materials.join(", ")}</p>
            <h4>Developers</h4>
            <p>${d.developers.map(
                x => htl.html`${x}<br>`
            )}</p>
            <h4>Contact</h4>
            <p>${d.contacts.length == 0 ? '-' : d.contacts.map(
                x => htl.html`<a href="${x.includes('@') ? 'mailto:' : ''}${x}">${x}</a><br>`
            )}</p>
            <h4>Comment</h4>
            <p>${d.comment == '' ? '-' : d.comment}</p>
        </div>`)}
</div>

---

## Data

### Filtered
```js
    display(filtered_catalogue)
```
### Original
```js
  display(catalogue)
```
