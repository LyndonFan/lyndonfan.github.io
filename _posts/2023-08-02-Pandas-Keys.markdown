---
layout: post
title:  A "Magic" Trick
date:   2023-08-02 23:45:00 +0100
categories: Python
---

_Hey kids, wanna see a function work and not work at the same time?_

(This article has an accompanying notebook and data file -- see them [here](LINKTBC)! )

## Set up

I was working with some data that only had names and information, but no ids. My job was to create that id. The data was ■■■■■■■■■■■■■■■■■■

Wait, looks like it's censored. Let me try again.

The data resembled some [magic cards](https://en.wikipedia.org/wiki/Magic:_The_Gathering) that I downloaded from the internet. The ids mysteriously went missing, so my first job was to create them. Here's what the data looked like:

```python
import warnings
warnings.filterwarnings('ignore')
import pandas as pd
import numpy as np
df = pd.read_csv('silver_bordered_set_cards.csv')
display(df.columns.tolist())
df[["name","set_name","border_color","oracle_text"]]
```

    ['name',
     'set_name',
     'border_color',
     'mana_cost',
     'type_line',
     'oracle_text',
     'power',
     'toughness',
     'loyalty',
     'collector_number',
     'watermark',
     'usd',
     'eur']

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>border_color</th>
      <th>oracle_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Goblin Bowling Team</td>
      <td>Unglued</td>
      <td>silver</td>
      <td>If Goblin Bowling Team would deal damage to a ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mise</td>
      <td>Unhinged</td>
      <td>silver</td>
      <td>Choose a nonland card name, then reveal the to...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eye to Eye</td>
      <td>Unhinged</td>
      <td>silver</td>
      <td>You and target creature's controller have a st...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>705</th>
      <td>Krark's Other Thumb</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>If you would roll a die, instead roll two of t...</td>
    </tr>
    <tr>
      <th>706</th>
      <td>Jamming Device</td>
      <td>Unstable</td>
      <td>borderless</td>
      <td>Whenever you crank Jamming Device, creatures t...</td>
    </tr>
    <tr>
      <th>707</th>
      <td>Who // What // When // Where // Why</td>
      <td>Unsanctioned</td>
      <td>silver</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>708</th>
      <td>Ninja</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>You may activate Ninja's augment ability any t...</td>
    </tr>
    <tr>
      <th>709</th>
      <td>Serpentine</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>silver</td>
      <td>Landfall — Whenever a land enters the battlefi...</td>
    </tr>
  </tbody>
</table>
<p>710 rows × 4 columns</p>
</div>

Each card had a name, mana cost, card type, and some (oracle) text. Each of them came from a set of cards, which may include some cards that have been printed before. I knew this collection of cards came from different sets, and reprints would occur in them.

Luckily, I knew cards with the same name are the same, even across different sets. To showcase my point,

```python
df.loc[
    df['name'].duplicated(keep=False), ['name','set_name']
].sort_values(by=['name','set_name'])
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>609</th>
      <td>AWOL</td>
      <td>Unhinged</td>
    </tr>
    <tr>
      <th>701</th>
      <td>AWOL</td>
      <td>Unhinged</td>
    </tr>
    <tr>
      <th>417</th>
      <td>AWOL</td>
      <td>Unsanctioned</td>
    </tr>
    <tr>
      <th>624</th>
      <td>Acornelia, Fashionable Filcher</td>
      <td>The List (Unfinity Foil Edition)</td>
    </tr>
    <tr>
      <th>650</th>
      <td>Acornelia, Fashionable Filcher</td>
      <td>Unsanctioned</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>X</td>
      <td>The List (Unfinity Foil Edition)</td>
    </tr>
    <tr>
      <th>683</th>
      <td>X</td>
      <td>Unstable</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Yet Another Aether Vortex</td>
      <td>The List (Unfinity Foil Edition)</td>
    </tr>
    <tr>
      <th>636</th>
      <td>Yet Another Aether Vortex</td>
      <td>Unhinged</td>
    </tr>
    <tr>
      <th>629</th>
      <td>Yet Another Aether Vortex</td>
      <td>Unsanctioned</td>
    </tr>
  </tbody>
</table>
<p>394 rows × 2 columns</p>
</div>

Picking the first name as an example, we can see the it's the same card by checking the mana cost, type line, and oracle text:

(we could do more, but I'm too lazy to type them all out)

```python
df.loc[
    df['name']=='AWOL',["oracle_text", "mana_cost", "type_line"]
].drop_duplicates()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>oracle_text</th>
      <th>mana_cost</th>
      <th>type_line</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>417</th>
      <td>Exile target attacking creature. Then remove i...</td>
      <td>{2}{W}</td>
      <td>Instant</td>
    </tr>
  </tbody>
</table>
</div>

So I can just use the name to generate the id (or just use the name itself), and call it a day. Right?

## The Catch

When I said the same card name means the same card anywhere, that's mostly true. It's true for black bordered cards, which are more "serious" and fit the game more. As you may have spotted earlier, some of the cards don't have a black border -- they are silver-bordered! These silver-bordered sets are more comedic in tone, take themselves less seriously, and are more wild in both flavour and mechanics.

This also means some cards with the same name have different text.

```python
df.loc[
    df["name"]=="Garbage Elemental",
    ["name", "set_name", "oracle_text"]
]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>114</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Frenzy 2 (Whenever this creature attacks and i...</td>
    </tr>
    <tr>
      <th>370</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>When Garbage Elemental enters the battlefield,...</td>
    </tr>
    <tr>
      <th>414</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Cascade (When you cast this spell, exile cards...</td>
    </tr>
    <tr>
      <th>421</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Battle cry (Whenever this creature attacks, ea...</td>
    </tr>
    <tr>
      <th>605</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Last strike (This creature deals combat damage...</td>
    </tr>
    <tr>
      <th>665</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Unleash (You may have this creature enter the ...</td>
    </tr>
  </tbody>
</table>
</div>

In fact, a good number of cards across the sets behave like above.

```python
unique_text_df = (
    df.groupby(
        ["name","set_name"], as_index=False
    )["oracle_text"].nunique()
)
unique_text_df[unique_text_df["oracle_text"]>1]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>36</th>
      <td>B.F.M. (Big Furry Monster)</td>
      <td>Unglued</td>
      <td>2</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Everythingamajig</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>2</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Everythingamajig</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
    <tr>
      <th>201</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
    <tr>
      <th>271</th>
      <td>Ineffable Blessing</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>2</td>
    </tr>
    <tr>
      <th>272</th>
      <td>Ineffable Blessing</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
    <tr>
      <th>312</th>
      <td>Knight of the Kitchen Sink</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
    <tr>
      <th>478</th>
      <td>Sly Spy</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
    <tr>
      <th>588</th>
      <td>Very Cryptic Command</td>
      <td>Unstable</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>

Let's save them to a new column so we can use it for later.

```python
df["has_variation"] = df["name"].isin(
    unique_text_df.loc[unique_text_df["oracle_text"]>1, "name"]
)
```

## The Function

Now we've found the catch, let's tackle the original problem of assigning ids. We can decide on the id function later, for now let's just put them into tuples.

<!-- I know there are some number of issues with this approach already, but bear with me -->

For those without variations, we only the name is enough. For those with variations, we have to include the oracle text as well. So here's what the keys/ids would look

```python
temp_keys = np.where(
    df["has_variation"],
    df[["has_variation","name","oracle_text"]].apply(tuple, axis=1),
    df[["has_variation","name"]].apply(tuple, axis=1),
)
temp_keys[:10]
```

    array([(False, 'Goblin Bowling Team'), (False, 'Mise'),
           (False, 'Steel Squirrel'), (False, 'Knight of the Widget'),
           (False, 'Eye to Eye'), (False, 'Split Screen'),
           (False, 'Six-y Beast'), (False, 'Handy Dandy Clone Machine'),
           (False, 'Aesthetic Consultation'), (False, 'Togglodyte')],
          dtype=object)

```python
temp_keys[105:115]
```

    array([(False, 'Gleemax'), (False, 'Keeper of the Sacred Word'),
           (True, 'Very Cryptic Command', 'Choose two —\\n• Untap two target permanents.\\n• Tap each permanent target player controls with exactly one word in its name.\\n• Discard all the cards in your hand, then draw that many cards.\\n• Return target instant or sorcery card from your graveyard to your h'),
           (False, 'Mirror Mirror'), (False, "Mary O'Kill"),
           (False, 'Flavor Judge'), (False, 'Gnome-Made Engine'),
           (False, 'Snickering Squirrel'), (False, 'B-I-N-G-O'),
           (True, 'Garbage Elemental', "Frenzy 2 (Whenever this creature attacks and isn't blocked, it gets +2/+0 until end of turn.)\\nGarbage Elemental can't be blocked by wordy creatures. (A creature is wordy if it has four or more lines of rules text.")],
          dtype=object)

Great! Let's chuck it in a function. I want to make the list of columns changable in different parts of the code, so let's put them into parameters. We also want to add typing for readability as well, and I don't want to use np.array as a type, so I've put the result into a pandas series.

```python
def gen_key(
    df, no_variation_cols: list[str], have_variation_cols: list[str]
) -> pd.Series:
    if "has_variation" not in no_variation_cols:
        no_variation_cols = ["has_variation"] + no_variation_cols
    if "has_variation" not in have_variation_cols:
        have_variation_cols = ["has_variation"] + have_variation_cols
    return pd.Series(np.where(
        df["has_variation"],
        df[have_variation_cols].apply(tuple, axis=1),
        df[no_variation_cols].apply(tuple, axis=1)
    ))
```

Wait, I just got word from my super that we're only worried about cards whose type line starts with "Artifact". No worries, let's add a filter and then run our newly constructed function:

```python
artifact_df = df[df["type_line"].str.startswith("Artifact")]
artifact_df["key"] = gen_key(artifact_df, ["name"], ["name", "oracle_text"])
artifact_df[["name","set_name","oracle_text","key"]].head(5)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>(False, Handy Dandy Clone Machine)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Split Screen</td>
      <td>Unstable</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Insufferable Syphon)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Handy Dandy Clone Machine</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>{2}, {T}: Create a 2/2 colorless Homunculus cr...</td>
      <td>(False, Do-It-Yourself Seraph)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Togglodyte</td>
      <td>Unhinged</td>
      <td>Togglodyte enters the battlefield turned on.\n...</td>
      <td>(False, Contraption Cannon)</td>
    </tr>
  </tbody>
</table>
</div>

Well, that doesn't look correct. Let me try again, but without the "Artifact" requirement:

```python
df["key2"] = gen_key(df, ["name"], ["name", "oracle_text"])
df[["name","set_name","oracle_text","key2"]].head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Goblin Bowling Team</td>
      <td>Unglued</td>
      <td>If Goblin Bowling Team would deal damage to a ...</td>
      <td>(False, Goblin Bowling Team)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mise</td>
      <td>Unhinged</td>
      <td>Choose a nonland card name, then reveal the to...</td>
      <td>(False, Mise)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>(False, Steel Squirrel)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>(False, Knight of the Widget)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eye to Eye</td>
      <td>Unhinged</td>
      <td>You and target creature's controller have a st...</td>
      <td>(False, Eye to Eye)</td>
    </tr>
  </tbody>
</table>
</div>

```python
df.loc[
    df["name"]=="Garbage Elemental",
    ["name","set_name","oracle_text","key2"]
]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>114</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Frenzy 2 (Whenever this creature attacks and i...</td>
      <td>(True, Garbage Elemental, Frenzy 2 (Whenever t...</td>
    </tr>
    <tr>
      <th>370</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>When Garbage Elemental enters the battlefield,...</td>
      <td>(True, Garbage Elemental, When Garbage Element...</td>
    </tr>
    <tr>
      <th>414</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Cascade (When you cast this spell, exile cards...</td>
      <td>(True, Garbage Elemental, Cascade (When you ca...</td>
    </tr>
    <tr>
      <th>421</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Battle cry (Whenever this creature attacks, ea...</td>
      <td>(True, Garbage Elemental, Battle cry (Whenever...</td>
    </tr>
    <tr>
      <th>605</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Last strike (This creature deals combat damage...</td>
      <td>(True, Garbage Elemental, Last strike (This cr...</td>
    </tr>
    <tr>
      <th>665</th>
      <td>Garbage Elemental</td>
      <td>Unstable</td>
      <td>Unleash (You may have this creature enter the ...</td>
      <td>(True, Garbage Elemental, Unleash (You may hav...</td>
    </tr>
  </tbody>
</table>
</div>

Wait, what?

In the actual code and data I was working on, all the above was buried inside 10 other different files and a lot of more transformations. It took me almost a day to narrow it down to this function, and a lot longer to figure out the issue with the above. I tried writing tests for it, all of which passed. I double checked the data, which looked to be fine. This little utility function was used in multiple places across files, and rewriting all of them would take significant time.

I was just about to give up when I finally identified the issue.

## The reveal

Nothing's wrong with values, the problem was with the index:

```python
gen_key(artifact_df, ["name"], ["name", "oracle_text"]).index
```

    RangeIndex(start=0, stop=145, step=1)

Compare this to the index of the `artifact_df`:

```python
artifact_df.index
```

    Index([  2,   3,   5,   7,   9,  17,  19,  20,  26,  28,
           ...
           668, 671, 681, 685, 691, 694, 702, 703, 704, 706],
          dtype='int64', length=145)

They don't align!

This is why the key for "Steel Squrriel" was `(False, "Split Screen")`. The card's index in the filtered dataframe was 2, but in the series it had an index of 0. At the same time, the third key in the series had an index of 2, so that's used instead.

This also means that the some elements in `artifact_df` supposedly won't have any keys, but now we know it's because its index doesn't appear in the series. We can even pinpoint to say it's all rows with `index >= len(artifact_df)`.

(I subtracted 10 to show the elements just before it)

```python
artifact_df.loc[
    artifact_df.index >= len(artifact_df)-10,
    ["name","set_name","oracle_text","key"]
].head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>141</th>
      <td>Handy Dandy Clone Machine</td>
      <td>Unstable</td>
      <td>{2}, {T}: Create a 2/2 colorless Homunculus cr...</td>
      <td>(False, World-Bottling Kit)</td>
    </tr>
    <tr>
      <th>144</th>
      <td>Time Machine</td>
      <td>Unhinged</td>
      <td>{T}: Exile Time Machine and target nontoken cr...</td>
      <td>(False, Jamming Device)</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Knight of the Kitchen Sink</td>
      <td>Unstable</td>
      <td>First strike, protection from black borders (N...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Riveting Rigger</td>
      <td>Unstable</td>
      <td>When Riveting Rigger enters the battlefield, y...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>152</th>
      <td>Knight of the Kitchen Sink</td>
      <td>Unstable</td>
      <td>First strike, protection from watermarks (Noth...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

Now we've understood what went wrong, let's fix it. The easiest fix is to not wrap the result in a pandas series -- no index, no problems!

```python
def gen_key_fixed_np(
    df, no_variation_cols: list[str], have_variation_cols: list[str]
) -> np.array:
    if "has_variation" not in no_variation_cols:
        no_variation_cols = ["has_variation"] + no_variation_cols
    if "has_variation" not in have_variation_cols:
        have_variation_cols = ["has_variation"] + have_variation_cols
    return np.where(
        df["has_variation"],
        df[have_variation_cols].apply(tuple, axis=1),
        df[no_variation_cols].apply(tuple, axis=1)
    )
```

```python
artifact_df = df[df["type_line"].str.startswith("Artifact")]
artifact_df["key3"] = gen_key_fixed_np(
    artifact_df, ["name"], ["name", "oracle_text"]
)
artifact_df[["name","set_name","oracle_text","key3"]].head(10)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>(False, Steel Squirrel)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>(False, Knight of the Widget)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Split Screen</td>
      <td>Unstable</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Handy Dandy Clone Machine</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>{2}, {T}: Create a 2/2 colorless Homunculus cr...</td>
      <td>(False, Handy Dandy Clone Machine)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Togglodyte</td>
      <td>Unhinged</td>
      <td>Togglodyte enters the battlefield turned on.\n...</td>
      <td>(False, Togglodyte)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Insufferable Syphon</td>
      <td>Unstable</td>
      <td>Whenever you crank Insufferable Syphon, target...</td>
      <td>(False, Insufferable Syphon)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Faerie Aerie</td>
      <td>Unstable</td>
      <td>Whenever you crank Faerie Aerie, create two 1/...</td>
      <td>(False, Faerie Aerie)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Do-It-Yourself Seraph</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>Flying\nWhenever Do-It-Yourself Seraph attacks...</td>
      <td>(False, Do-It-Yourself Seraph)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Split Screen</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Contraption Cannon</td>
      <td>Unstable</td>
      <td>{2}, Sacrifice Contraption Cannon: It deals da...</td>
      <td>(False, Contraption Cannon)</td>
    </tr>
  </tbody>
</table>
</div>

Alternatively, if we wanted to have it return a pandas series, we can suppliment it with the index of the dataframe.

```python
def gen_key_fixed_pd(
    df, no_variation_cols: list[str], have_variation_cols: list[str]
) -> pd.Series:
    if "has_variation" not in no_variation_cols:
        no_variation_cols = ["has_variation"] + no_variation_cols
    if "has_variation" not in have_variation_cols:
        have_variation_cols = ["has_variation"] + have_variation_cols
    return pd.Series(
        np.where(
            df["has_variation"],
            df[have_variation_cols].apply(tuple, axis=1),
            df[no_variation_cols].apply(tuple, axis=1)
        ),
        index = df.index
    )
```

```python
artifact_df = df[df["type_line"].str.startswith("Artifact")]
artifact_df["key4"] = gen_key_fixed_pd(
    artifact_df, ["name"], ["name", "oracle_text"]
)
artifact_df[["name","set_name","oracle_text","key4"]].head(10)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>(False, Steel Squirrel)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>(False, Knight of the Widget)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Split Screen</td>
      <td>Unstable</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Handy Dandy Clone Machine</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>{2}, {T}: Create a 2/2 colorless Homunculus cr...</td>
      <td>(False, Handy Dandy Clone Machine)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Togglodyte</td>
      <td>Unhinged</td>
      <td>Togglodyte enters the battlefield turned on.\n...</td>
      <td>(False, Togglodyte)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Insufferable Syphon</td>
      <td>Unstable</td>
      <td>Whenever you crank Insufferable Syphon, target...</td>
      <td>(False, Insufferable Syphon)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Faerie Aerie</td>
      <td>Unstable</td>
      <td>Whenever you crank Faerie Aerie, create two 1/...</td>
      <td>(False, Faerie Aerie)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Do-It-Yourself Seraph</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>Flying\nWhenever Do-It-Yourself Seraph attacks...</td>
      <td>(False, Do-It-Yourself Seraph)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Split Screen</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Contraption Cannon</td>
      <td>Unstable</td>
      <td>{2}, Sacrifice Contraption Cannon: It deals da...</td>
      <td>(False, Contraption Cannon)</td>
    </tr>
  </tbody>
</table>
</div>

Phew! We've finally fixed the problem. Now we can get back on track and focus on whatever we were on before.

## The takeaways

_What have we learned today, kids?_

_"Don't use np.where!"_

Well, not quite.

Do use it when you're dealing with numpy arrays, but with pandas, you have to be a bit more mindful about the indexes, especially with dataframes being filtered and transformed everywhere. If you don't remember whether a dataframe has been filtered or it's a fresh copy, better make the function not aware of its index, or just reset it if you are cautious.

In fact, the point about filtered and copied dataframes applies beyond indexes. When manipulating data, some operations create copies, while others only create views. Here, the filtering created a view of the dataframe, while the initial implementation of `gen_key` creates a new series. As you saw, they don't exactly mesh with each other well.

Oh, and don't use apply like I did. Find another way to do it, such as with `.iloc` and merges, or like this:

```python
def gen_key_no_apply(
    df, no_variation_cols: list[str], have_variation_cols: list[str]
) -> pd.Series:
    if "has_variation" not in no_variation_cols:
        no_variation_cols = ["has_variation"] + no_variation_cols
    if "has_variation" not in have_variation_cols:
        have_variation_cols = ["has_variation"] + have_variation_cols
    return pd.Series(
        np.where(
            df["has_variation"],
            df.set_index(have_variation_cols).index,
            df.set_index(no_variation_cols).index,
        ),
        index = df.index
    )
```

```python
artifact_df = df[df["type_line"].str.startswith("Artifact")]
artifact_df["key5"] = gen_key_no_apply(
    artifact_df, ["name"], ["name", "oracle_text"]
)
artifact_df[["name","set_name","oracle_text","key5"]].head(10)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>set_name</th>
      <th>oracle_text</th>
      <th>key5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>(False, Steel Squirrel)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>(False, Knight of the Widget)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Split Screen</td>
      <td>Unstable</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Handy Dandy Clone Machine</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>{2}, {T}: Create a 2/2 colorless Homunculus cr...</td>
      <td>(False, Handy Dandy Clone Machine)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Togglodyte</td>
      <td>Unhinged</td>
      <td>Togglodyte enters the battlefield turned on.\n...</td>
      <td>(False, Togglodyte)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Insufferable Syphon</td>
      <td>Unstable</td>
      <td>Whenever you crank Insufferable Syphon, target...</td>
      <td>(False, Insufferable Syphon)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Faerie Aerie</td>
      <td>Unstable</td>
      <td>Whenever you crank Faerie Aerie, create two 1/...</td>
      <td>(False, Faerie Aerie)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Do-It-Yourself Seraph</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>Flying\nWhenever Do-It-Yourself Seraph attacks...</td>
      <td>(False, Do-It-Yourself Seraph)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Split Screen</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Split Screen)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Contraption Cannon</td>
      <td>Unstable</td>
      <td>{2}, Sacrifice Contraption Cannon: It deals da...</td>
      <td>(False, Contraption Cannon)</td>
    </tr>
  </tbody>
</table>
</div>
