import random

# Define the rooms and their descriptions
rooms = {
    'Hall': 'You are in the hall. There is a door to the north.',
    'Kitchen': 'You are in the kitchen. There is a door to the south.',
    'Bedroom': 'You are in the bedroom. There is a door to the east.',
    'Bathroom': 'You are in the bathroom. There is a door to the west.'
}

# Define the items in each room
items = {
    'Hall': ['key'],
    'Kitchen': ['knife', 'apple'],
    'Bedroom': ['book', 'lamp'],
    'Bathroom': ['soap']
}

# Define the starting room
current_room = 'Hall'

# Define the player's inventory
inventory = []

# Define a function to display the current room and its contents
def display_room():
    print('\n' + rooms[current_room])
    print('Items in the room:', ', '.join(items[current_room]))

# Define a function to move the player to a new room
def move(direction):
    global current_room
    if direction in rooms[current_room]:
        for room in rooms:
            if direction in rooms[room]:
                current_room = room
                display_room()
                return
    print("You can't go that way.")

# Define a function to pick up an item and add it to the player's inventory
def get(item):
    global current_room
    if item in items[current_room]:
        items[current_room].remove(item)
        inventory.append(item)
        print(item, 'added to inventory.')
        display_room()
    else:
        print(item, 'not found in room.')

# Define a function to display the player's inventory
def display_inventory():
    print('\nInventory:', ', '.join(inventory))

# Define the main game loop
while True:
    # Display the current room and its contents
    display_room()

    # Ask the player for input
    command = input('\nWhat do you do? ').split()

    # Parse the input
    verb = command[0].lower()
    if len(command) > 1:
        noun = command[1].lower()
    else:
        noun = ''

    # Execute the player's command
    if verb == 'go':
        move(noun)
    elif verb == 'get':
        get(noun)
    elif verb == 'inventory':
        display_inventory()
    else:
        print('Invalid command.')
