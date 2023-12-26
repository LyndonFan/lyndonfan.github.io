---
layout: post
title:  "Merry Christmas!"
date:   2023-12-27 22:00:00 +0800
categories: Meta
---

<style>
    .tree {
        font-size: 20px;
        font-family: 'source code pro', monospace;
        align-items: center;
        text-align: center;
        display: block;
    }
    .tree > .row {
        vertical-align: top;
        margin: 0;
    }
</style>

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

    function generateTree(layers) {
        const treeContainer = document.querySelector('.tree');
        for (let i = 1; i <= layers; i++) {
            const row = document.createElement('div');
            row.classList.add('row');
            row.id = `row-${i}`;
            let symbols = [];
            for (let j = 0; j < 2 * i - 1; j++) {
                symbols.push(getRandomSymbol());
            }
            symbols[0] = ">";
            symbols[2 * i - 2] = "<";
            for (let j = 1; j < 2 * i - 2; j++) {
                if (symbols[j] !== ".") {
                    if ((symbols[j-1] === ">") || (symbols[j-1] === "<")) {
                        continue;
                    }
                    symbols[j] = "."
                }
                let newSymbol = Math.random() > 0.5 ? ">" : "<"
                while ((j < 2 * i - 2) && (symbols[j] === ".")) {
                    symbols[j] = newSymbol;
                    j++;
                }
            }
            for (let j = 0; j < 2 * i - 1; j++) {
                const symbol = document.createElement('span');
                symbol.innerHTML = symbols[j];
                row.appendChild(symbol);
            }
            treeContainer.appendChild(row);
        }
    }

    generateTree(25);
</script>
