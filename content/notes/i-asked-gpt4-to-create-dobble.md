---
title: "I Asked Gpt4 to help me create a Dobble card deck"
date: 2023-03-26T21:38:14+02:00
draft: true
---

# I asked GPT4 to help me create a Dobble card deck

My first try of GPT 4 from [OpenAi](https://openai.com).
My prompts are quite messy but the interlocutor was
very helpful.

---

```prompt
Hello, I want to create a gobble card game.
How much symbols do I need to create a set of 50 cards ?
```

In a Gobble card game (also known as Dobble or Spot It), each card has a unique 
combination of symbols, and each pair of cards shares exactly one symbol in
common. To create a Gobble card game with 50 cards, you'll need to determine the
number of symbols per card and the total number of unique symbols required.

The game is based on projective geometry and can be represented by the formula:

n(n - 1) + 1 = total number of unique symbols

where n is the number of symbols per card.

For a Gobble card game with 50 cards, you will have:

50 = n(n - 1) + 1

Solving this equation for n, you'll find that there isn't a whole number 
solution. The closest solutions are n = 7 and n = 8, which result in 43 and 57
unique symbols, respectively.

So, if you want to create a Gobble card game with 50 cards, you can either:

1. Create a game with 7 symbols per card and 43 unique symbols. In this case,
   you will have 7 extra cards that you can use as additional variants, bonus 
   cards, or even for replacement cards in case some get damaged or lost.

2. Create a game with 8 symbols per card and 57 unique symbols. In this case, 
   you will need to remove 7 cards to have exactly 50 cards, but the game 
   mechanics will still work correctly.

```prompt
I have a folder with 43 image files in it, can you give me a python script
that read this folder and prints an array (the card set) of array (each 
row is a list of the file names that will be the symbols on the cards)  ?
```

Sure, here's a Python script that reads a folder containing 43 image files and
generates a card set with 7 symbols per card, where each row represents a card: 

```code-pink
import os
from itertools import combinations

def generate_gobble_cards(folder_path):
    # Read the folder and get the image filenames
    image_filenames = [filename for filename in os.listdir(folder_path) if filename.endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp'))]
    
    if len(image_filenames) != 43:
        raise ValueError('The folder should contain exactly 43 image files.')

    # Generate cards
    cards = []
    n = 7

    # Add the first card
    cards.append(image_filenames[:n])

    # Generate combinations of the remaining symbols
    remaining_symbols = image_filenames[n:]
    combos = list(combinations(remaining_symbols, n - 1))

    # Iterate through the symbols of the first card
    for i, symbol in enumerate(cards[0]):
        for combo in combos:
            if symbol not in combo:
                new_card = [symbol] + list(combo)
                cards.append(new_card)
                combos.remove(combo)

    return cards

folder_path = 'path/to/your/folder'  # Replace with the path to your folder containing 43 image files
card_set = generate_gobble_cards(folder_path)

# Print the card set
for idx, card in enumerate(card_set):
    print(f'Card {idx + 1}:', card)
```

Make sure to replace 'path/to/your/folder' with the actual path to the folder 
containing your 43 image files. This script assumes that the folder contains
exactly 43 image files. The script will generate and print the card set, where
each card is a list of image filenames representing the symbols on the card.


```prompt
Can you modify the script so it creates me from the same folder only a
set of 10 cards with a minimal number of symbols ?
```

Sure! Here's a modified version of the script to generate a set of 10 cards with
a minimal number of symbols:

```code-pink
import os
from itertools import combinations

def generate_gobble_cards(folder_path, num_cards):
    # Read the folder and get the image filenames
    image_filenames = [filename for filename in os.listdir(folder_path) if filename.endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp'))]
    
    if len(image_filenames) < num_cards:
        raise ValueError('The folder should contain at least as many image files as the number of cards required.')

    # Determine the number of symbols per card
    n = 2
    while (n * (n - 1) + 1) < num_cards:
        n += 1

    # Generate cards
    cards = []

    # Add the first card
    cards.append(image_filenames[:n])

    # Generate combinations of the remaining symbols
    remaining_symbols = image_filenames[n:]
    combos = list(combinations(remaining_symbols, n - 1))

    # Iterate through the symbols of the first card
    for i, symbol in enumerate(cards[0]):
        for combo in combos:
            if symbol not in combo:
                new_card = [symbol] + list(combo)
                cards.append(new_card)
                combos.remove(combo)
                break

        if len(cards) >= num_cards:
            break

    return cards

folder_path = 'path/to/your/folder'  # Replace with the path to your folder containing image files
num_cards = 10
card_set = generate_gobble_cards(folder_path, num_cards)

# Print the card set
for idx, card in enumerate(card_set):
    print(f'Card {idx + 1}:', card)
```  

Make sure to replace 'path/to/your/folder' with the actual path to the folder
containing your image files. This script will generate and print a card set with
the specified number of cards (10 in this case) and a minimal number of symbols
per card. Keep in mind that the folder should contain at least as many image 
files as the number of cards required.

```prompt
Can you modify the script in order to a new directory called cards 
and for each card in the card_set, create a new image file in 
the cards directory using its symbols and imagemagick.
```

Certainly! To create a new directory called 'cards' and save each card as an 
image file using ImageMagick, you'll first need to install the Python library 
wand which is a ctypes-based simple ImageMagick binding for Python. 
You can install it using pip:

```code-pink
pip install Wand
```  

After installing the wand library, you can modify the script as follows:

```code-pink
import os
from itertools import combinations
from wand.image import Image
from wand.display import display

def generate_gobble_cards(folder_path, num_cards):
    # Read the folder and get the image filenames
    image_filenames = [filename for filename in os.listdir(folder_path) if filename.endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp'))]
    
    if len(image_filenames) < num_cards:
        raise ValueError('The folder should contain at least as many image files as the number of cards required.')

    # Determine the number of symbols per card
    n = 2
    while (n * (n - 1) + 1) < num_cards:
        n += 1

    # Generate cards
    cards = []

    # Add the first card
    cards.append(image_filenames[:n])

    # Generate combinations of the remaining symbols
    remaining_symbols = image_filenames[n:]
    combos = list(combinations(remaining_symbols, n - 1))

    # Iterate through the symbols of the first card
    for i, symbol in enumerate(cards[0]):
        for combo in combos:
            if symbol not in combo:
                new_card = [symbol] + list(combo)
                cards.append(new_card)
                combos.remove(combo)
                break

        if len(cards) >= num_cards:
            break

    return cards

def create_card_images(folder_path, card_set):
    cards_dir = 'cards'
    os.makedirs(cards_dir, exist_ok=True)

    for idx, card in enumerate(card_set):
        with Image(filename=os.path.join(folder_path, card[0])) as base_image:
            for symbol in card[1:]:
                with Image(filename=os.path.join(folder_path, symbol)) as symbol_image:
                    base_image.composite(symbol_image, left=0, top=0)

            base_image.save(filename=os.path.join(cards_dir, f'card_{idx + 1}.png'))
            # Uncomment the following line if you want to display the created card image
            # display(base_image)

folder_path = 'path/to/your/folder'  # Replace with the path to your folder containing image files
num_cards = 10
card_set = generate_gobble_cards(folder_path, num_cards)
create_card_images(folder_path, card_set)

```

This script will create a new directory called 'cards' and save each card as an
image file (PNG) in that directory. The symbols are composited on top of each
other using ImageMagick through the wand library. You can adjust the left
and top parameters in the base_image.composite() method to change the positions
of the symbols. Note that this simple script assumes all image symbols have the
same size and transparent background. You may need to adjust the script to
handle different image sizes and backgrounds.

Please replace 'path/to/your/folder' with the actual path to the folder 
containing your image files.

```prompt
Dobble game card are round, can you modify the script so in the 
create_card_images function it creates a black circle of diameter 1000,
resizes the symbols to fit inside and distributes them in trigonometric
order around the center with their top oriented to the center 
of the circle ?
```

Of course! To create circular Dobble game cards with symbols resized and 
distributed evenly in trigonometric order, we can modify 
the create_card_images function as follows:

```code-pink
import math
from wand.color import Color

def create_card_images(folder_path, card_set):
    cards_dir = 'cards'
    os.makedirs(cards_dir, exist_ok=True)
    
    card_diameter = 1000
    symbol_size = card_diameter // 4

    for idx, card in enumerate(card_set):
        with Image(width=card_diameter, height=card_diameter, background=Color('black')) as base_image:
            angle_step = 360 / len(card)

            for i, symbol in enumerate(card):
                angle = math.radians(i * angle_step)
                x_offset = (card_diameter - symbol_size) // 2 * math.cos(angle)
                y_offset = (card_diameter - symbol_size) // 2 * math.sin(angle)

                with Image(filename=os.path.join(folder_path, symbol)) as symbol_image:
                    symbol_image.transform(resize=f"{symbol_size}x{symbol_size}")
                    symbol_image.rotate(-i * angle_step)
                    x = (card_diameter - symbol_image.width) // 2 + int(x_offset)
                    y = (card_diameter - symbol_image.height) // 2 - int(y_offset)
                    base_image.composite(symbol_image, left=x, top=y)

            base_image.save(filename=os.path.join(cards_dir, f'card_{idx + 1}.png'))
            # Uncomment the following line if you want to display the created card image
            # display(base_image)
```

This modified create_card_images function creates a black circle with a diameter 
of 1000, resizes the symbols to fit inside, and distributes them evenly in 
trigonometric order around the center with their top oriented to the center of
the circle.

The rest of the script remains unchanged:

```code-pink
import os
from itertools import combinations
from wand.image import Image
from wand.display import display

# ... (generate_gobble_cards function)

folder_path = 'path/to/your/folder'
num_cards = 10
card_set = generate_gobble_cards(folder_path, num_cards)
create_card_images(folder_path, card_set)
```

Replace 'path/to/your/folder' with the actual path to the folder containing your
image files. The script will create and save the circular Dobble game cards with
resized and distributed symbols in the 'cards' directory as before.
