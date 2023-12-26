---
layout: post
title:  "Merry Christmas!"
date:   2023-12-26 22:00:00 +0800
categories: Meta
---

<style>
    .tree {
        font-size: 20px;
        font-family: 'Source Code Pro', 'Roboto', monospace;
        text-align: center;
        display: block;
        background-color: black;
    }
    .tree > .row {
        margin: 0;
        padding: 0;
        line-height: 1.25em;
        color: green;
    }
    .tree-light {
        color: yellow;
        text-shadow: 0 0 5px yellow;
    }
    .tree-ornament0 {
        color: red;
    }
    .tree-ornament1 {
        color: blue;
    }
    .tree-ornament2 {
        color: orange;
    }
    .tree-trunk {
        color: brown;
    }
</style>

Merry Christmas! Enjoy this ASCII Christmas tree inspired by [Advent of Code](adventofcode.com). Go check it out!

<div class="tree"></div>

<script>
    function getRandomSymbol() {
        const symbols = [".", "*", "@", "O", "o"];
        const weights = [0.72, 0.07, 0.07, 0.07, 0.07];
        const random = Math.random();

        let cumulativeWeight = 0;
        for (let i = 0; i < symbols.length; i++) {
            cumulativeWeight += weights[i];
            if (random < cumulativeWeight) {
                return symbols[i];
            }
        }
    }

    function generateRow(rowLength) {
        if (rowLength === 1) {return ["."];}
        if (rowLength === 2) {return [".", "."];}
        let numOrnaments = 0;
        let row = [];
        while (numOrnaments < (rowLength - 2) / 3){
            row = [];
            for (let i = 0; i < rowLength - 2; i++) {
                let symbol = getRandomSymbol();
                if (symbol !== ".") {numOrnaments++;}
                row.push(symbol);
                if ((symbol !== ".") && (i < rowLength - 3)){
                    row.push(".");
                    i++
                }
            }
        }
        for (let j = 0; j < rowLength - 2; j++) {
            if (row[j] !== ".") {
                if ((j === 0) || (row[j-1] === ">") || (row[j-1] === "<")) {
                    continue;
                }
                row[j] = "."
            }
            let newSymbol = Math.random() > 0.5 ? ">" : "<"
            while ((j < rowLength - 2) && (row[j] === ".")) {
                row[j] = newSymbol;
                j++;
            }
        }
        row.unshift(">");
        row.push("<");
        return row;
    }

    function generateTree(layers) {
        const treeContainer = document.querySelector('.tree');
        const topLightRows = [
            "|",
            "\\|/",
            "--*--",
        ];
        for (let i = 0; i < 3; i++) {
            const row = document.createElement('p');
            row.classList.add('row');
            row.id = `light-row-${i}`;
            const rowText = document.createElement('span');
            rowText.innerHTML = topLightRows[i];
            rowText.classList.add("tree-light");
            row.appendChild(rowText);
            treeContainer.appendChild(row);
        }
        for (let i = 2; i <= layers; i++) {
            const row = document.createElement('p');
            row.classList.add('row');
            row.id = `row-${i}`;
            let symbols = generateRow(2*i-1);
            const symbolClasses = {
                "*": "tree-light",
                "@": "tree-ornament0",
                "O": "tree-ornament1",
                "o": "tree-ornament2",
            };
            for (let j = 0; j < 2 * i - 1; j++) {
                const symbol = document.createElement('span');
                symbol.innerHTML = symbols[j];
                if (symbols[j] in symbolClasses){
                    symbol.classList.add(symbolClasses[symbols[j]]);
                }
                row.appendChild(symbol);
            }
            treeContainer.appendChild(row);
        }
        const bottomTrunkRows = [
            "|&nbsp;&nbsp;&nbsp;|",
            "|&nbsp;&nbsp;&nbsp;|",
            "---|---|---",
        ]
        for (let i = 0; i < 3; i++) {
            const row = document.createElement('p');
            row.classList.add('row');
            row.id = `trunk-row-${i}`;
            const rowText = document.createElement('span');
            rowText.innerHTML = bottomTrunkRows[i];
            rowText.classList.add("tree-trunk");
            row.appendChild(rowText);
            treeContainer.appendChild(row);
        }
    }

    generateTree(25);
</script>
