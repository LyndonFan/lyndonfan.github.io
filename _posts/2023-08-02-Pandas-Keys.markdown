---
layout: post
title:  A "Magic" Trick
date:   2023-08-02 22:00:00 +0100
categories: Python
---

_Hey kids, wanna see a function work and not work at the same time?_

## Set up

I was working with some data that only had names and information, but no ids. My job was to create that id. The data was ■■■■■■■■■■■■■■■■■■

Wait, looks like it's censored. Let me try again.

The data resembled some [magic cards](LINKTOMTGWIKI) that I downloaded from the internet. The ids mysteriously went missing, so my first job was to create them. Here's what the data looked like:

```python
import warnings
warnings.filterwarnings('ignore')
import pandas as pd
import numpy as np
df = pd.read_csv('silver_bordered_set_cards.csv')
df.head()
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
      <th>border_color</th>
      <th>mana_cost</th>
      <th>type_line</th>
      <th>oracle_text</th>
      <th>power</th>
      <th>toughness</th>
      <th>loyalty</th>
      <th>collector_number</th>
      <th>watermark</th>
      <th>usd</th>
      <th>eur</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Goblin Bowling Team</td>
      <td>Unglued</td>
      <td>silver</td>
      <td>{3}{R}</td>
      <td>Creature — Goblin</td>
      <td>If Goblin Bowling Team would deal damage to a ...</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>44</td>
      <td>NaN</td>
      <td>0.20</td>
      <td>0.35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mise</td>
      <td>Unhinged</td>
      <td>silver</td>
      <td>{U}</td>
      <td>Instant</td>
      <td>Choose a nonland card name, then reveal the to...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>38</td>
      <td>NaN</td>
      <td>0.23</td>
      <td>0.65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Steel Squirrel</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>{2}</td>
      <td>Artifact Creature — Squirrel</td>
      <td>Whenever you roll a 5 or higher on a die, Stee...</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>162</td>
      <td>NaN</td>
      <td>0.30</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Knight of the Widget</td>
      <td>Unstable</td>
      <td>silver</td>
      <td>{2}{W}</td>
      <td>Artifact Creature — Cyborg Knight</td>
      <td>Vigilance\nKnight of the Widget's power and to...</td>
      <td>*</td>
      <td>*</td>
      <td>NaN</td>
      <td>13</td>
      <td>orderofthewidget</td>
      <td>0.10</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eye to Eye</td>
      <td>Unhinged</td>
      <td>silver</td>
      <td>{2}{B}</td>
      <td>Instant</td>
      <td>You and target creature's controller have a st...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>54</td>
      <td>NaN</td>
      <td>0.28</td>
      <td>0.48</td>
    </tr>
  </tbody>
</table>
</div>

Each card had a name, mana cost, card type, and some (oracle) text. Each of them came from a set of cards, which may include some cards that have been printed before. I knew this collection of cards came from different sets, and reprints would occur in them.

Luckily, I knew cards with the same name are the same, even across different sets. To showcase my point,

```python
print(df['name'].duplicated().any())
df.loc[df['name'].duplicated(keep=False), ['name','set_name']].sort_values(by=['name','set_name'])
```

    True

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
df.loc[df['name']=='AWOL',["oracle_text", "mana_cost", "type_line"]].drop_duplicates()
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
df.loc[df["name"]=="Garbage Elemental", ["name", "set_name", "oracle_text"]].drop_duplicates()
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
unique_text_df = df.groupby(["name","set_name"], as_index=False, dropna=False)["oracle_text"].nunique()
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
df["has_variation"] = df["name"].isin(unique_text_df.loc[unique_text_df["oracle_text"]>1, "name"])
original_df = df.copy() # don't mind me, just saving a copy in case of issues down the line
```

## The Function

Now we've found the catch, let's tackle the original problem of assigning ids. We can decide on the id function later, for now let's just put them into tuples.

<!-- I know there are some number of issues with this approach already, but bear with me -->

For those without variations, we only the name is enough. For those with variations, we have to include the oracle text as well. So here's what the keys/ids would look

```python
np.where(
    df["has_variation"],
    df[["has_variation","name","oracle_text"]].apply(tuple, axis=1),
    df[["has_variation","name"]].apply(tuple, axis=1),
)
```

    array([(False, 'Goblin Bowling Team'), (False, 'Mise'),
           (False, 'Steel Squirrel'), (False, 'Knight of the Widget'),
           (False, 'Eye to Eye'), (False, 'Split Screen'),
           (False, 'Six-y Beast'), (False, 'Handy Dandy Clone Machine'),
           (False, 'Aesthetic Consultation'), (False, 'Togglodyte'),
           (False, 'Organ Harvest'), (False, 'X'),
           (False, 'Cardboard Carapace'), (False, 'Novellamental'),
           (False, 'Moniker Mage'), (False, 'The Grand Calcutron'),
           (False, 'GO TO JAIL'), (False, 'Insufferable Syphon'),
           (False, 'Humming-'), (False, 'Faerie Aerie'),
           (False, 'Do-It-Yourself Seraph'), (False, 'Monkey Monkey Monkey'),
           (False, 'Handcuffs'), (False, 'Richard Garfield, Ph.D.'),
           (False, 'Smart Ass'), (False, 'Rare-B-Gone'),
           (False, 'Split Screen'), (False, "Ass Whuppin'"),
           (False, 'Contraption Cannon'), (False, 'Booster Tutor'),
           (False, 'Old Fogey'), (False, 'Standing Army'),
           (False, 'Goblin Haberdasher'), (False, 'Island'),
           (False, 'Genetic Recombinator'), (False, 'Ordinary Pony'),
           (False, 'Standing Army'), (False, 'Do-It-Yourself Seraph'),
           (False, 'Spy Eye'), (False, 'Artful Looter'),
           (False, 'Aesthetic Consultation'), (False, 'Faerie Spy'),
           (False, 'Goblin S.W.A.T. Team'), (False, 'Island'),
           (False, 'Magic Word'), (False, 'Widget Contraption'),
           (False, 'Plains'),
           (True, 'Very Cryptic Command', "Choose two —\\n• Return target permanent to its controller's hand.\\n• Draw two cards, then discard a card.\\n• Change the target of target spell with a single target.\\n• Turn over target nontoken creat"),
           (False, 'Denied!'), (False, 'Forest'),
           (False, 'Steady-Handed Mook'), (False, 'Creature Guy'),
           (False, 'Hypnotic Swirly Disc'), (False, 'Sorry'),
           (False, 'Clocknapper'), (False, "Look at Me, I'm the DCI"),
           (False, 'Clever Combo'), (False, 'First Pick'),
           (False, 'Staff of the Letter Magus'), (False, 'Secret Base'),
           (False, 'Dirty Rat'), (False, 'Ghazbán Ogress'),
           (False, 'Burning Cinder Fury of Crimson Chaos Fire'),
           (False, 'Jack-in-the-Mox'),
           (True, 'Everythingamajig', '{2}, {T}: Move a counter from one permanent onto another. If the second permanent refers to any kind of counter, the moved counter becomes one of those counters. Otherwise, it becomes a +1/+1 counter.\\n{3}, {T}: Put a +1/+1 counter on target creature.\\n{4}, {T}: Proliferate. (Choose any number of permanents and/or players, then give each another counter of each kind already there'),
           (False, 'Construct'), (False, 'Baron Von Count'),
           (False, 'Forest'), (False, 'Blurry Beeble'),
           (False, "Land Aid '04"), (False, 'Swamp'),
           (False, 'Alexander Clamilton'), (False, 'Clocknapper'),
           (False, 'Novellamental'), (False, 'Clam Session'),
           (False, 'Crafty Octopus'), (False, 'Giant Fan'),
           (False, 'Flock of Rabid Sheep'), (False, 'Bursting Beebles'),
           (False, 'Plains'), (False, 'S.N.E.A.K. Dispatcher'),
           (False, 'Dogsnail Engine'), (False, 'Enraged Killbot'),
           (False, 'Target Minotaur'), (False, 'Letter Bomb'),
           (False, 'Infernius Spawnington III, Esq.'),
           (False, 'Bronze Calendar'), (False, 'When Fluffy Bunnies Attack'),
           (False, 'Goblin Secret Agent'), (False, 'Form of the Squirrel'),
           (False, 'Plains'), (False, 'Fowl Play'), (False, 'Goblin Tutor'),
           (False, 'Sundering Fork'), (False, 'Water Gun Balloon Game'),
           (False, 'Skull Saucer'), (False, 'Yet Another Aether Vortex'),
           (False, 'Steamflogger Service Rep'), (False, 'Adorable Kitten'),
           (False, 'Get a Life'), (False, 'Sheep'), (False, 'Growth Spurt'),
           (False, 'Gnomeball Machine'), (False, 'Hoisted Hireling'),
           (False, 'Auto-Key'), (False, 'Gleemax'),
           (False, 'Keeper of the Sacred Word'),
           (True, 'Very Cryptic Command', 'Choose two —\\n• Untap two target permanents.\\n• Tap each permanent target player controls with exactly one word in its name.\\n• Discard all the cards in your hand, then draw that many cards.\\n• Return target instant or sorcery card from your graveyard to your h'),
           (False, 'Mirror Mirror'), (False, "Mary O'Kill"),
           (False, 'Flavor Judge'), (False, 'Gnome-Made Engine'),
           (False, 'Snickering Squirrel'), (False, 'B-I-N-G-O'),
           (True, 'Garbage Elemental', "Frenzy 2 (Whenever this creature attacks and isn't blocked, it gets +2/+0 until end of turn.)\\nGarbage Elemental can't be blocked by wordy creatures. (A creature is wordy if it has four or more lines of rules text."),
           (False, 'Three-Headed Goblin'), (False, 'Thopter // Thopter'),
           (True, 'Very Cryptic Command', "Choose two —\\n• Draw a card from an opponent's library.\\n• Copy target instant or sorcery spell. You may choose new targets for the copy.\\n• Until end of turn, target creature loses all abilities and becomes a blue Frog with base power and toughness 1/1.\\n• Create a 1/1 colorless Gnome artifact creature to"),
           (False, 'The Big Idea'), (False, 'Bad Ass'),
           (False, "Ladies' Knight"), (False, 'Hoisted Hireling'),
           (False, 'Water Gun Balloon Game'), (False, 'Hard Hat Area'),
           (False, 'Mer Man'), (False, 'Surgeon General Commander'),
           (False, 'Duh'), (False, 'Goblin Haberdasher'), (False, 'Time Out'),
           (False, 'Head Banger'), (False, 'Wild Crocodile'),
           (False, 'Painiac'), (False, 'Vile Bile'), (False, 'My First Tome'),
           (False, 'Rapid Prototyper'), (False, 'Bad Ass'),
           (False, 'Super-Duper Death Ray'), (False, 'Phyrexian Librarian'),
           (False, 'Clambassadors'), (False, 'Common Courtesy'),
           (False, 'Beast in Show'), (False, 'Handy Dandy Clone Machine'),
           (False, 'Hangman'), (False, 'Monkey Monkey Monkey'),
           (False, 'Time Machine'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from black borders (Nothing with a black border can block, target, deal damage to, or attach to this creature.)'),
           (False, 'Hungry Hungry Heifer'),
           (False, 'Steamflogger of the Month'), (False, 'Cramped Bunker'),
           (False, 'Riveting Rigger'), (False, 'The Grand Calcutron'),
           (False, 'Atinlay Igpay'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from watermarks (Nothing with a watermark can block, target, deal damage to, or attach to this creature.)'),
           (False, 'Jackknight'), (False, 'Mother of Goons'),
           (False, 'Landfill'), (False, 'Timmy, Power Gamer'),
           (False, 'Chicken à la King'), (False, 'Touch and Go'),
           (False, 'Phoebe, Head of S.N.E.A.K.'),
           (False, 'Infernal Spawn of Infernal Spawn of Evil'),
           (False, 'Cardpecker'), (False, 'Flaccify'),
           (False, 'Infinity Elemental'), (False, 'Gift Horse'),
           (False, 'Spirit // Spirit'), (False, 'Mountain'),
           (False, 'Steamflogger Temp'), (False, 'Defective Detective'),
           (False, 'Hazmat Suit (Used)'), (False, 'Wordmail'),
           (False, 'Bureaucracy'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose white-bordered or silver-bordered.\\nWhenever a creature with the chosen border enters the battlefield under your control, draw a card'),
           (False, 'Frankie Peanuts'), (False, 'Grusilda, Monster Masher'),
           (False, 'Wild Crocodile'), (False, 'Bronze Calendar'),
           (False, 'Mountain'), (False, 'Cheap Ass'), (False, 'Dragon'),
           (False, 'Super Secret Tech'), (False, 'Spirit of the Season'),
           (False, 'Farewell to Arms'), (False, 'Border Guardian'),
           (False, 'Shaggy Camel'), (False, 'Common Iguana'),
           (False, 'Oaken Power Suit'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from loose lips (Nothing with an open mouth in its artwork can block, target, deal damage to, or attach to this creature.)'),
           (False, 'Bumbling Pangolin'), (False, 'Richard Garfield, Ph.D.'),
           (False, 'As Luck Would Have It'), (False, 'Number Crunch'),
           (False, 'Extremely Slow Zombie'), (False, 'Kill! Destroy!'),
           (False, 'capital offense'), (False, 'The Countdown Is at One'),
           (False, 'Rogue'), (False, 'Frazzled Editor'),
           (False, 'Infernal Spawn of Evil'), (False, 'Wrench-Rigger'),
           (False, 'Blacker Lotus'), (False, 'Amateur Auteur'),
           (False, 'Clay Pigeon'), (False, 'Buzzing Whack-a-Doodle'),
           (False, 'Mana Flair'), (False, 'Feisty Stegosaurus'),
           (False, "Look at Me, I'm the DCI"), (False, 'Drawn Together'),
           (False, "Ol' Buzzbark"), (False, 'Curious Killbot'),
           (False, 'Targeting Rocket'), (False, 'Goblin Mime'),
           (False, 'Laughing Hyena'), (False, 'Double Header'),
           (False, 'Squirrel'), (False, 'Bloodletter'),
           (False, 'Sword of Dungeons \\u0026 Dr'), (False, 'Hydradoodle'),
           (False, 'Island'), (False, 'Graveyard Busybody'),
           (False, 'Dr. Julius Jumblemorph'),
           (False, 'Pointy Finger of Doom'), (False, 'Monkey-'),
           (False, 'Incoming!'), (False, 'Time Machine'),
           (False, 'Orcish Paratroopers'), (False, 'Gimme Five'),
           (True, 'Sly Spy', "Whenever Sly Spy deals combat damage to a player, you may destroy target creature facing right in its art. (Creatures without faces don't face anywhere.)"),
           (False, "I'm Rubber, You're Glue"), (False, 'Party Crasher'),
           (False, 'The Countdown Is at One'), (False, 'Zombie // Zombie'),
           (False, '_____'), (False, 'Persecute Artist'),
           (False, 'Frankie Peanuts'), (False, 'Rock Lobster'),
           (False, 'Socketed Sprocketer'), (False, 'Record Store'),
           (False, 'Our Market Research Shows That Players Like Really Long Card Names So We Made this Card to Have the Absolute Longest Card Name Ever Elemental'),
           (False, 'Meddling Kids'), (False, 'Blast from the Past'),
           (True, 'Sly Spy', 'Whenever Sly Spy deals combat damage to a player, that player reveals their hand. You choose a card from it with the longest name. That player discards that card.'),
           (False, 'Vampire // Vampire'),
           (False, 'Infernius Spawnington III, Esq.'),
           (False, 'Touch and Go'), (False, 'Finders, Keepers'),
           (False, 'Lackey Recycler'), (False, 'Common Iguana'),
           (False, 'Psychic Network'), (False, 'Beast // Beast'),
           (False, 'Strategy, Schmategy'), (False, 'Richard Garfield, Ph.D.'),
           (False, 'The Ultimate Nightmare of Wizards of the Coast® Customer Service'),
           (False, 'Free-for-All'), (False, 'Slaying Mantis'),
           (False, 'Chicken Egg'), (False, 'Face to Face'),
           (False, 'Zombified'), (False, 'Magical Hacker'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose a number.\\nWhenever a creature with exactly the chosen number of words in its name enters the battlefield under your control, draw a card. (Hyphenated words are one word.'),
           (False, 'Stinging Scorpion'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose Flavorful or Bland.\\n• Flavorful — Whenever a creature with flavor text enters the battlefield under your control, draw a card.\\n• Bland — Whenever a creature without flavor text enters the battlefield under your control, draw a car'),
           (False, 'Swamp'), (False, 'Box of Free-Range Goblins'),
           (False, 'Magic Word'), (False, 'Joyride Rigger'),
           (False, 'Secret Base'), (False, 'Syr Cadian, Knight Owl'),
           (False, 'Goblin'), (False, 'Rules Lawyer'),
           (False, 'Laughing Hyena'), (False, 'Paper Tiger'),
           (False, "Jester's Sombrero"), (False, 'Mana Screw'),
           (False, 'Ambiguity'), (False, 'Masterful Ninja'),
           (False, 'Brushstroke Paintermage'), (False, 'Six-y Beast'),
           (False, 'Tread Mill'), (False, 'Elemental // Elemental'),
           (False, 'Double Take'), (False, 'Farewell to Arms'),
           (False, 'Labro Bot'), (False, 'Suspicious Nanny'),
           (False, 'City of Ass'), (False, "Mons's Goblin Waiters"),
           (False, 'Elemental // Elemental'), (False, 'Question Elemental?'),
           (False, 'Novellamental'), (False, 'Goblin Slingshot'),
           (False, 'Mountain'), (False, 'Shoe Tree'), (False, 'Forest'),
           (False, 'Boomstacker'), (False, 'Miss Demeanor'),
           (False, 'Pippa, Duchess of Dice'), (False, 'Amateur Auteur'),
           (False, 'Over My Dead Bodies'), (False, 'Numbing Jellyfish'),
           (False, 'Hydradoodle'), (False, 'Ordinary Pony'),
           (False, 'Beast in Show'), (False, 'Spatula of the Ages'),
           (False, 'Target Minotaur'), (False, 'Gleemax'),
           (False, 'Dictation Quillograph'), (False, 'Prismatic Wardrobe'),
           (False, 'Punctuate'), (False, 'Hot Fix'), (False, 'Plains'),
           (False, 'Dumb Ass'), (False, 'Humming-'),
           (False, 'Wall of Fortune'), (False, 'Collector Protector'),
           (False, "Urza's Hot Tub"), (False, 'Rock Lobster'),
           (False, 'B.O.B. (Bevy of Beebles)'), (False, 'Summon the Pack'),
           (False, "Krark's Other Thumb"), (False, 'Storm Crow'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose an artist.\\nWhenever a creature with art by the chosen artist enters the battlefield under your control, draw a card'),
           (False, 'Target Minotaur'), (False, 'Kindly Cognician'),
           (False, 'Arms Depot'), (False, 'Mad Science Fair Project'),
           (False, 'Grusilda, Monster Masher'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from odd collector numbers (Nothing with an odd collector number can block, target, deal damage to, or attach to this creature.)'),
           (False, 'Oddly Uneven'), (False, 'Swamp'),
           (False, 'Chittering Doom'), (False, 'Hammer Helper'),
           (False, '\\"Ach! Hans, Run!'), (False, 'Sauté'),
           (False, 'Fraction Jackson'), (False, 'Rhino-'),
           (False, 'Selfie Preservation'),
           (False, 'Entirely Normal Armchair'), (False, 'Squirrel Dealer'),
           (False, 'Who // What // When // Where // Why'),
           (False, 'Knight of the Hokey Pokey'), (False, 'Delighted Killbot'),
           (False, 'Chipper Chopper'), (False, 'Frazzled Editor'),
           (False, "Urza's Science Fair Project"), (False, 'Shoe Tree'),
           (False, 'Saproling // Saproling'), (False, 'Avatar of Me'),
           (False, 'Phoebe, Head of S.N.E.A.K.'), (False, 'Avatar of Me'),
           (False, 'Applied Aeronautics'), (False, 'Mountain'),
           (False, 'Zombie'), (False, 'Quick-Stick Lick Trick'),
           (False, "Mary O'Kill"), (False, 'Red-Hot Hottie'),
           (False, 'Fat Ass'), (False, 'Voracious Vacuum'),
           (False, 'Stinging Scorpion'), (False, 'Little Girl'),
           (False, 'Division Table'), (False, 'Steamflogger Boss'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose white-bordered or silver-bordered.\\nWhenever a creature with the chosen border enters the battlefield under your control, draw a card'),
           (False, 'Goblin S.W.A.T. Team'), (False, 'Tainted Monkey'),
           (False, 'Elvish Impersonators'), (False, 'Staying Power'),
           (False, 'Mother Kangaroo'), (False, 'The Big Idea'),
           (False, 'Charm School'),
           (True, 'Sly Spy', "Whenever Sly Spy deals combat damage to a player, that player loses a finger until Sly Spy leaves the battlefield. (The finger is chosen by its owner and can't roll dice or touch cards.)"),
           (True, 'Garbage Elemental', "When Garbage Elemental enters the battlefield, it assembles a Contraption. (Put the top card of your Contraption deck face up onto one of your sprockets.)\\nUndying (When this creature dies, if it had no +1/+1 counters on it, return it to the battlefield under its owner's control with a +1/+1 counter on it."),
           (False, "Teacher's Pet"), (False, 'Chivalrous Chevalier'),
           (False, 'Wet Willie of the Damned'), (False, 'Symbol Status'),
           (False, "Granny's Payback"), (False, 'Staying Power'),
           (False, 'Willing Test Subject'), (False, 'Man of Measure'),
           (False, "Now I Know My ABC's"),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose odd or even.\\nWhenever a creature with a collector number of the chosen value enters the battlefield under your control, draw a card'),
           (False, 'Zombie Fanboy'), (False, 'Island'),
           (False, 'Enter the Dungeon'), (False, 'Sex Appeal'),
           (False, 'Dr. Julius Jumblemorph'), (False, 'Snickering Squirrel'),
           (False, 'Squirrel Farm'), (False, 'Remodel'),
           (False, 'Animate Library'), (False, 'Infernal Spawn of Evil'),
           (False, 'Secret Base'),
           (True, 'Very Cryptic Command', "Choose two —\\n• Switch target creature's power and toughness until end of turn.\\n• Target creature can't be blocked this turn.\\n• Draw a card. If that card's art is by Wayne England, you may reveal it and draw another card.\\n• Assemble a Contrapt"),
           (False, 'Common Courtesy'), (False, 'Clue // Clue'),
           (False, 'Assquatch'), (False, 'Jalum Grifter'),
           (False, 'Kindslaver'), (False, "R\\u0026D's Secret"),
           (False, 'Lobe Lobber'), (False, 'Name Dropping'),
           (False, 'Amateur Auteur'), (False, 'Steamflogger of the Month'),
           (False, 'Secret Base'), (False, 'Incite Insight'),
           (False, 'Beast in Show'), (False, 'Old Guard'),
           (False, 'Jumbo Imp'), (False, 'Modular Monstrosity'),
           (False, 'Alexander Clamilton'), (False, 'Avatar of Me'),
           (False, 'Proper Laboratory Attire'),
           (True, 'Everythingamajig', '{1}, {T}: Scry 2.\\n{1}, {T}, Pay 1 life: Create a 0/1 white Goat creature token.\\n{7}, {T}, Sacrifice Everythingamajig: Choose target player. At the beginning of the next end step, exchange life totals with that player, exchange control of all permanents you and that player control, and exchange cards in your hands, cards in your libraries, and cards in your graveyard'),
           (False, 'Strategy, Schmategy'),
           (True, 'Garbage Elemental', 'Cascade (When you cast this spell, exile cards from the top of your library until you exile a nonland card that costs less. You may cast it without paying its mana cost. Put the exiled cards on the bottom of your library in a random order.)\\nWhen Garbage Elemental enters the battlefield, roll a six-sided die. Garbage Elemental deals damage equal to the result to target opponent or planeswalker'),
           (False, 'Old-Fashioned Vampire'),
           (False, 'Richard Garfield, Ph.D.'), (False, 'AWOL'),
           (False, 'Cardpecker'), (False, 'Fascist Art Director'),
           (False, 'Oddly Uneven'),
           (True, 'Garbage Elemental', 'Battle cry (Whenever this creature attacks, each other attacking creature gets +1/+0 until end of turn.)\\nWhen Garbage Elemental enters the battlefield, roll two six-sided dice. Create a number of 1/1 red Goblin creature tokens equal to the difference between those results'),
           (False, 'Chaos Confetti'), (False, 'Side Quest'),
           (False, 'When Fluffy Bunnies Attack'), (False, 'Boomflinger'),
           (False, 'Blast from the Past'), (False, 'Paper Tiger'),
           (False, 'Growth Spurt'), (False, 'Side to Side'),
           (False, 'Refibrillator'), (False, 'Mine, Mine, Mine!'),
           (False, 'Mother Kangaroo'), (False, 'Head to Head'),
           (True, 'B.F.M. (Big Furry Monster)', "You must cast both B.F.M. cards to put\\nleaves the battlefield, sacrifice the other.\\nB.F.M. can't be blocked except by three "),
           (False, 'Target Minotaur'), (False, 'Blast from the Past'),
           (False, 'Gluetius Maximus'), (False, 'Cramped Bunker'),
           (False, 'Pointy Finger of Doom'), (False, 'Topsy Turvy'),
           (False, "Look at Me, I'm R\\u"), (False, 'Five-Finger Discount'),
           (False, 'Cheatyface'), (False, 'Goblin Tutor'),
           (False, 'Flavor Judge'), (False, 'Half-Squirrel, Half-'),
           (False, 'Better Than One'), (False, 'Multi-Headed'),
           (False, 'Squirrel'), (False, 'Old Fogey'),
           (False, 'Earl of Squirrel'),
           (True, 'Everythingamajig', '{1}: Flip a coin. If you win the flip, add {C}{C}. Activate only as an instant.\\n{3}, {T}: Target player discards a card. Activate only during your turn.\\n{X}: Everythingamajig becomes an X/X Construct artifact creature until end of tur'),
           (False, 'Animate Library'), (False, 'Syr Cadian, Knight Owl'),
           (False, "Mons's Goblin Waiters"), (False, 'Mouth to Mouth'),
           (False, 'Gerrymandering'), (False, 'Brainiac'),
           (False, 'Pygmy Giant'), (False, 'Cogmentor'), (False, 'Mox Lotus'),
           (False, 'Eager Beaver'), (False, 'Spike, Tournament Grinder'),
           (False, 'More or Less'), (False, 'Bat-'), (False, 'Soldier'),
           (False, 'Elvish Impersonators'), (False, 'Rules Lawyer'),
           (False, 'Surgeon General Commander'),
           (False, 'Extremely Slow Zombie'),
           (False, "Volrath's Motion Sensor"),
           (False, 'Extremely Slow Zombie'), (False, 'Lexivore'),
           (False, 'Loose Lips'),
           (True, 'Everythingamajig', '{2}, {T}: Draw a card. Activate only if you have no cards in hand.\\n{8}, {T}: You gain 10 life.\\n{4}, {T}, Sacrifice Everythingamajig: You may put a silver-bordered or acorn permanent card from your hand onto the battlefiel'),
           (False, 'Forest'), (False, 'The Fallen Apart'),
           (False, 'Kindslaver'), (False, 'Robo-'), (False, 'Clam-I-Am'),
           (False, 'Duplication Device'), (False, 'Working Stiff'),
           (False, 'Temp of the Damned'), (False, 'Aerial Toastmaster'),
           (False, 'Half-Squirrel, Half-'), (False, 'Team Spirit'),
           (False, 'First Come, First Served'), (False, 'Graveyard Busybody'),
           (False, 'Graphic Violence'), (False, 'Novellamental'),
           (False, 'Dirty Rat'), (False, 'Once More with Feeling'),
           (False, 'Save Life'), (False, 'Incite Insight'),
           (False, 'Sword of Dungeons \\u0026 Dr'),
           (False, 'Overt Operative'), (False, 'Rings a Bell'),
           (False, 'Midlife Upgrade'), (False, 'Pegasus'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose a rarity.\\nWhenever a creature with the chosen rarity enters the battlefield under your control, draw a card'),
           (False, 'Knight of the Hokey Pokey'), (False, 'Uktabi Kong'),
           (False, 'Angelic Rocket'),
           (True, 'Sly Spy', 'Whenever Sly Spy deals combat damage to a player, roll a six-sided die. That player loses life equal to the result.'),
           (False, 'Steam-Powered'),
           (True, 'Ineffable Blessing', 'As Ineffable Blessing enters the battlefield, choose Flavorful or Bland.\\n• Flavorful — Whenever a creature with flavor text enters the battlefield under your control, draw a card.\\n• Bland — Whenever a creature without flavor text enters the battlefield under your control, draw a car'),
           (False, 'Turbo-Thwacking Auto-Hammer'),
           (False, 'Half-Kitten, Half-'), (False, 'Frankie Peanuts'),
           (False, 'Sword of Dungeons \\u0026 Dr'), (False, "Ol' Buzzbark"),
           (False, 'Slaying Mantis'), (False, 'Watermarket'),
           (False, 'Curse of the Fire Penguin // Curse of the Fire Penguin Creature'),
           (False, 'Deadly Poison Sampler'), (False, 'Hot Fix'),
           (False, 'Work a Double'), (False, 'It That Gets Left Hanging'),
           (False, 'Infinity Elemental'), (False, "Look at Me, I'm R\\u"),
           (False, 'Abstract Iguanart'), (False, 'Subcontract'),
           (False, 'Ground Pounder'), (False, 'Booster Tutor'),
           (False, 'Earl of Squirrel'), (False, 'Thud-for-Duds'),
           (False, 'Bee-Bee Gun'), (False, 'Goat'),
           (False, 'Jack-in-the-Mox'), (False, 'Optical Optimizer'),
           (False, 'Island'), (False, 'Over My Dead Bodies'),
           (False, 'Super-Duper Death Ray'), (False, 'Inflation Station'),
           (True, 'Everythingamajig', "{T}: Add one mana of any color.\\n{1}, {T}: Say the flavor text on a card in your hand. Target opponent guesses that card's name. You may reveal that card. If you do and your opponent guessed wrong, draw a card.\\n{8}, {T}: Everythingamajig deals 12 damage to target creatur"),
           (False, 'Steamfloggery'), (False, 'Poultrygeist'),
           (False, 'Free-Range Chicken'), (False, 'Earl of Squirrel'),
           (False, 'Scissors Lizard'), (False, 'Half-Orc, Half-'),
           (False, 'Beast in Show'), (False, 'Guest List'),
           (False, 'Twiddlestick Charger'),
           (True, 'Everythingamajig', 'Sacrifice a land: You gain 2 life.\\nSacrifice a creature: Add {C}{C}.\\n{2}, Discard a card: Search your library for a card that shares a complete word in its name with the name of the discarded card, reveal it, put it into your hand, then shuffl'),
           (False, 'Emcee'), (False, 'Druid of the Sacred Beaker'),
           (False, '\\"Rumors of My Death . . .'), (False, 'Plains'),
           (False, 'Squirrel Farm'), (False, 'Checks and Balances'),
           (False, 'Old Fogey'), (False, 'Framed!'),
           (False, 'Summon the Pack'), (False, 'Deadhead'),
           (False, 'Topsy Turvy'), (False, 'Success!'), (False, 'Supersize'),
           (False, 'Mesa Chicken'), (False, 'Rod of Spanking'),
           (False, 'The Cheese Stands Alone'), (False, 'Painiac'),
           (False, 'Sacrifice Play'), (False, 'Pet Project'),
           (True, 'B.F.M. (Big Furry Monster)', 'B.F.M. onto the battlefield. If one B.F.M. card\\nmore creatures'),
           (False, 'Stone-Cold Basilisk'), (False, 'As Luck Would Have It'),
           (False, "Zzzyxas's Abyss"), (False, 'Adorable Kitten'),
           (False, 'Neural Network'), (False, 'Letter Bomb'),
           (False, 'Top-Secret Tunnel'), (False, 'Greater Morphling'),
           (False, 'Carnivorous Death-Parrot'), (False, 'GO TO JAIL'),
           (True, 'Very Cryptic Command', "Choose two —\\n• Scry 3.\\n• Create a 2/2 black Rogue creature token with menace.\\n• Add or subtract 1 or one from a number or number word on target spell or permanent until end of turn.\\n• Return all artifacts target player controls to their owner's h"),
           (False, "Ashnod's Coupon"), (False, 'Clock of DOOOOOOOOOOOOM!'),
           (False, 'Stet, Draconic Proofreader'), (False, 'Underdome'),
           (False, 'Dual Doomsuits'), (False, 'Greater Morphling'),
           (False, 'Enter the Dungeon'), (False, 'Poultrygeist'),
           (False, 'Jumbo Imp'), (False, 'Modular Monstrosity'),
           (False, 'Old Guard'),
           (True, 'Very Cryptic Command', "Choose two —\\n• Counter target black-bordered spell.\\n• Return target creature to its owner's hand.\\n• Untap each permanent you control with a watermark.\\n• Roll two six-sided dice. Target player mills X cards, where X is the total of those resu"),
           (False, 'Emcee'), (False, 'Hurloon Wrangler'), (False, 'Gus'),
           (False, 'Goblin Mime'), (False, 'Shellephant'),
           (False, 'Carnivorous Death-Parrot'), (False, 'Watermarket'),
           (False, 'Jackknight'), (False, 'Gnome'),
           (False, 'Blast from the Past'), (False, 'B-I-N-G-O'),
           (False, 'Goblin Bookie'), (False, 'Emcee'), (False, 'Ow'),
           (False, 'B.O.B. (Bevy of Beebles)'), (False, 'Deal Damage'),
           (True, 'Garbage Elemental', "Last strike (This creature deals combat damage after creatures without last strike.)\\nBattalion — Whenever Garbage Elemental and at least two other creatures attack, target creature can't block this turn"),
           (False, 'Infernal Spawn of Infernal Spawn of Evil'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from two-word names (Nothing with exactly two words in its name can block, target, deal damage to, or attach to this creature. Hyphenated words are one word.)'),
           (True, 'Everythingamajig', '{2}, {T}: Move a counter from one permanent onto another. If the second permanent refers to any kind of counter, the moved counter becomes one of those counters. Otherwise, it becomes a +1/+1 counter.\\n{3}, {T}: Put a +1/+1 counter on target creature.\\n{4}, {T}: Proliferate. (Choose any number of permanents and/or players, then give each another counter of each kind already there'),
           (False, 'AWOL'), (False, 'Inhumaniac'), (False, 'Angel // Angel'),
           (False, 'Censorship'), (False, 'Stop That'), (False, 'Swamp'),
           (False, 'Pippa, Duchess of Dice'), (False, 'Baron Von Count'),
           (False, 'Big Boa Constrictor'), (False, 'Double Play'),
           (False, 'Three-Headed Goblin'), (False, 'Spell Counter'),
           (False, 'Rocket-Powered Turbo Slug'),
           (False, 'Timmy, Power Gamer'), (False, 'Urza, Academy Headmaster'),
           (False, 'Acornelia, Fashionable Filcher'), (False, 'B-I-N-G-O'),
           (False, 'My First Tome'), (False, 'Free-Range Chicken'),
           (True, 'Sly Spy', "Whenever Sly Spy deals combat damage to a player, you may destroy target creature facing left in its art. (Creatures without faces don't face anywhere.)"),
           (False, 'Yet Another Aether Vortex'),
           (False, 'Despondent Killbot'), (False, 'By Gnome Means'),
           (False, 'By Gnome Means'),
           (False, 'Mandatory Friendship Shackles'), (False, 'Old Fogey'),
           (False, 'Hammer Jammer'), (False, 'Yet Another Aether Vortex'),
           (False, 'Enter the Dungeon'), (False, 'Question Elemental?'),
           (False, 'Keeper of the Sacred Word'),
           (False, 'Infernal Spawn of Infernal Spawn of Evil'),
           (False, 'Mox Lotus'), (False, 'Better Than One'),
           (False, 'Stamp of Approval'), (False, 'Accessories to Murder'),
           (False, 'Angelic Rocket'), (False, 'Double Cross'),
           (True, 'Everythingamajig', '{1}, {T}: Scry 2.\\n{1}, {T}, Pay 1 life: Create a 0/1 white Goat creature token.\\n{7}, {T}, Sacrifice Everythingamajig: Choose target player. At the beginning of the next end step, exchange life totals with that player, exchange control of all permanents you and that player control, and exchange cards in your hands, cards in your libraries, and cards in your graveyard'),
           (False, 'Krazy Kow'), (False, 'Inhumaniac'),
           (False, 'Acornelia, Fashionable Filcher'), (False, 'Spark Fiend'),
           (False, 'Ricochet'), (False, 'Really Epic Punch'),
           (False, 'Necro-Impotence'), (False, 'Cheatyface'),
           (False, 'Double Dip'),
           (True, 'Sly Spy', 'Whenever Sly Spy deals combat damage to a player, that player reveals the top card of their library. Put that card into your hand and you lose life equal to its mana value.'),
           (False, 'Mer Man'), (False, 'Dispatch Dispensary'),
           (False, 'Double Deal'), (False, 'Circle of Protection: Art'),
           (False, 'Mountain'), (False, 'Entirely Normal Armchair'),
           (False, 'Strutting Turkey'),
           (True, 'Garbage Elemental', "Unleash (You may have this creature enter the battlefield with a +1/+1 counter on it. It can't block as long as it has a +1/+1 counter on it.)\\nEach creature you control with any kind of counter on it has art menace. (They can't be blocked except by creatures with two or more visible figures in their art."),
           (False, 'Chicken à la King'), (False, 'Johnny, Combo Player'),
           (False, 'Toy Boat'), (False, 'Swamp'), (False, 'Serpentine'),
           (False, "Urza's Contact Lenses"),
           (False, "Erase (Not the Urza's Legacy One)"),
           (False, 'Amateur Auteur'), (False, 'Spike, Tournament Grinder'),
           (False, 'Half-Shark, Half-'), (False, 'Hammerfest Boomtacular'),
           (False, 'Crow Storm'), (False, 'Elvish House Party'),
           (False, 'Spell Suck'), (False, 'Squirrel-Powered Scheme'),
           (False, 'Wall of Fortune'), (False, 'S.N.O.T.'), (False, 'X'),
           (False, 'Just Desserts'), (False, 'Buzz Buggy'),
           (False, 'Masterful Ninja'), (False, 'Skull Saucer'),
           (False, 'Forest'), (False, 'Johnny, Combo Player'),
           (False, 'Infinity Elemental'), (False, 'Scissors Lizard'),
           (False, 'Bosom Buddy'), (False, 'Duh'),
           (True, 'Knight of the Kitchen Sink', 'First strike, protection from even collector numbers (Nothing with an even collector number can block, target, deal damage to, or attach to this creature.)'),
           (False, 'Time Out'), (False, 'Secret Base'),
           (False, "Ass Whuppin'"), (False, 'Goblin'),
           (False, 'Extremely Slow Zombie'), (False, 'Staying Power'),
           (False, 'AWOL'), (False, 'World-Bottling Kit'),
           (False, 'Mana Screw'), (False, 'Sap Sucker'),
           (False, "Krark's Other Thumb"), (False, 'Jamming Device'),
           (False, 'Who // What // When // Where // Why'), (False, 'Ninja'),
           (False, 'Serpentine')], dtype=object)

Great! Let's chuck it in a function. I want to make the list of columns changable in different parts of the code, so let's put them into parameters. We also want to add typing for readability as well, and I don't want to use np.array as a type, so I've put the result into a pandas series.

```python
def gen_key(df, no_variation_cols: list[str], have_variation_cols: list[str]) -> pd.Series:
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
artifact_df[["name","set_name","oracle_text","key"]].head(10)
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
    <tr>
      <th>17</th>
      <td>Insufferable Syphon</td>
      <td>Unstable</td>
      <td>Whenever you crank Insufferable Syphon, target...</td>
      <td>(False, Blurry Beeble)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Faerie Aerie</td>
      <td>Unstable</td>
      <td>Whenever you crank Faerie Aerie, create two 1/...</td>
      <td>(False, Dogsnail Engine)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Do-It-Yourself Seraph</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>Flying\nWhenever Do-It-Yourself Seraph attacks...</td>
      <td>(False, Enraged Killbot)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Split Screen</td>
      <td>The List (Unfinity Foil Edition)</td>
      <td>When Split Screen enters the battlefield, shuf...</td>
      <td>(False, Auto-Key)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Contraption Cannon</td>
      <td>Unstable</td>
      <td>{2}, Sacrifice Contraption Cannon: It deals da...</td>
      <td>(False, Water Gun Balloon Game)</td>
    </tr>
  </tbody>
</table>
</div>

Well, that doesn't look correct. Let me try again, but without the "Artifact" requirement:

```python
df["key2"] = gen_key(df, ["name"], ["name", "oracle_text"])
display(df[["name","set_name","oracle_text","key2"]].head())
display(df.loc[df["name"]=="Garbage Elemental",["name","set_name","oracle_text","key2"]])
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
gen_key(artifact_df, ["name"], ["name", "oracle_text"])
```

    0                                (False, Steel Squirrel)
    1                          (False, Knight of the Widget)
    2                                  (False, Split Screen)
    3                     (False, Handy Dandy Clone Machine)
    4                                    (False, Togglodyte)
                                 ...
    140    (True, Knight of the Kitchen Sink, First strik...
    141                          (False, World-Bottling Kit)
    142                                  (False, Mana Screw)
    143                                  (False, Sap Sucker)
    144                              (False, Jamming Device)
    Length: 145, dtype: object

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
artifact_df.loc[artifact_df.index >= len(artifact_df)-10, ["name","set_name","oracle_text","key"]]
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
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>694</th>
      <td>Knight of the Kitchen Sink</td>
      <td>Unstable</td>
      <td>First strike, protection from even collector n...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>702</th>
      <td>World-Bottling Kit</td>
      <td>Unhinged</td>
      <td>{5}, Sacrifice World-Bottling Kit: Choose a Ma...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>703</th>
      <td>Mana Screw</td>
      <td>Unhinged</td>
      <td>{1}: Flip a coin. If you win the flip, add {C}...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>704</th>
      <td>Sap Sucker</td>
      <td>Unstable</td>
      <td>Whenever you crank Sap Sucker, add {G}. Until ...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>706</th>
      <td>Jamming Device</td>
      <td>Unstable</td>
      <td>Whenever you crank Jamming Device, creatures t...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>112 rows × 4 columns</p>
</div>

Now we've understood what went wrong, let's fix it. The easiest fix is to not wrap the result in a pandas series -- no index, no problems!

```python
def gen_key_fixed_np(df, no_variation_cols: list[str], have_variation_cols: list[str]) -> np.array:
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
artifact_df["key3"] = gen_key_fixed_np(artifact_df, ["name"], ["name", "oracle_text"])
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
def gen_key_fixed_pd(df, no_variation_cols: list[str], have_variation_cols: list[str]) -> pd.Series:
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
artifact_df["key4"] = gen_key_fixed_pd(artifact_df, ["name"], ["name", "oracle_text"])
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

_Don't use `np.where`!_

Well, not quite.

Do use it when you're dealing with numpy arrays, but with pandas, you have to be a bit more mindful about the indexes, especially with dataframes being filtered and transformed everywhere. If you don't remember whether a dataframe has been filtered or it's a fresh copy, better make the function not aware of its index, or just reset it if you are cautious.

Oh, and don't use apply like I did. Find another way to do it:

```python
def gen_key_no_apply(df, no_variation_cols: list[str], have_variation_cols: list[str]) -> pd.Series:
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
artifact_df = df[df["type_line"].str.startswith("Artifact")]
artifact_df["key5"] = gen_key_no_apply(artifact_df, ["name"], ["name", "oracle_text"])
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
