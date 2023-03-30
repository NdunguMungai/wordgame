# wordgame
import random

# Define the items that can be found in the game
items = {
    'sword': {
        'name': 'Sword',
        'description': 'A shiny sword. It looks very sharp.'
    },
    'potion': {
        'name': 'Potion',
        'description': 'A glowing potion. You feel healthier just by looking at it.'
    },
    'key': {
        'name': 'Key',
        'description': 'A rusty key. It looks like it might unlock something important.'
    }
}

# Define the rooms in the game
rooms = {
    'start': {
        'name': 'Starting Room',
        'description': 'You are in a dark room. There is a door to the north.',
        'exits': {
            'north': 'corridor'
        }
    },
    'corridor': {
        'name': 'Corridor',
        'description': 'You are in a long corridor. There are doors to the north and south.',
        'exits': {
            'north': 'treasure_room',
            'south': 'start'
        }
    },
    'treasure_room': {
        'name': 'Treasure Room',
        'description': 'You are in a room filled with treasure! There is a door to the south.',
        'exits': {
            'south': 'corridor'
        },
        'items': ['sword', 'potion']
    }
}

# Define the player's initial state
player = {
    'current_room': 'start',
    'inventory': []
}

# Define a function to print the current room description and any items in it
def print_room():
    current_room = rooms[player['current_room']]
    print(current_room['name'])
    print(current_room['description'])
    if 'items' in current_room:
        print('You see the following items:')
        for item_name in current_room['items']:
            item = items[item_name]
            print(f"{item['name']}: {item['description']}")

# Define a function to handle player input
def process_input(input_str):
    input_str = input_str.lower()
    if input_str == 'help':
        print('Commands:')
        print('  help: Print this help message')
        print('  inventory: Print your inventory')
        print('  look: Print the description of the current room')
        print('  north/south/east/west: Move in the specified direction')
        print('  take [item]: Take an item from the current room')
        print('  use [item]: Use an item from your inventory')
    elif input_str == 'inventory':
        print('Your inventory:')
        if len(player['inventory']) == 0:
            print('  (empty)')
        else:
            for item_name in player['inventory']:
                item = items[item_name]
                print(f"  {item['name']}: {item['description']}")
    elif input_str == 'look':
        print_room()
    elif input_str in ['north', 'south', 'east', 'west']:
        move_player(input_str)
    elif input_str.startswith('take '):
        item_name = input_str.split(' ', 1)[1]
        take_item(item_name)
    elif input_str.startswith('use '):
        item_name = input_str.split(' ', 1)[1]
        use_item(item_name)
    else:
        print('Invalid command. Type "help" for a list of commands.')
#Define a function to move the player to a new room
def move_player(direction):
current_room = rooms[player['current_room']]
if direction in current_room['exits']:
new_room = current_room['exits'][direction]
player['current_room'] = new_room
print_room()
else:
print('You cannot move in that direction.')

#Define a function to take an item from the current room and add it to the player's inventory
def take_item(item_name):
current_room = rooms[player['current_room']]
if 'items' in current_room and item_name in current_room['items']:
player['inventory'].append(item_name)
current_room['items'].remove(item_name)
print(f"You take the {item_name}.")
else:
print(f"You cannot take the {item_name}.")

#Define a function to use an item from the player's inventory
def use_item(item_name):
if item_name in player['inventory']:
if item_name == 'key' and player['current_room'] == 'corridor':
print('You use the key to unlock the door to the treasure room!')
rooms['corridor']['exits']['north'] = 'treasure_room'
player['inventory'].remove('key')
else:
print(f"You use the {item_name}. Nothing happens.")
else:
print(f"You do not have the {item_name} in your inventory.")

def play_game():
print('Welcome to the game!')
print('Type "help" for a list of commands.')
print_room()
while True:
command = input('> ')
process_input(command)

#Start the game
play_game()
