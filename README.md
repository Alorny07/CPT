# CPT
CPT code for Grade 11 Computer Science
import random, time, copy, replit


print()
print('Welcome to MineSweeper v.3.0!')
print('*****************************')

def reset():
    
    print('''
    For instructions on how to play, type 'I'
    Start to play, type 'P'
''')

    choice = input('Type here: ').upper()

    if choice == 'I':
        replit.clear()
        print("MineSweeper is the game that you should determine the locations of 10 bombs which are randomly placed in a 9x9 grid.")
        print("Type in the coordinates of a square, e.g. A5.")
        print("If the square you choose has the boom, you lose. Otherwise, the number of bombs surrounding that square will be shwon in that square.")
        print("If that number is a 0, the squares around it will be 'opened' automatically that means no boom there.")
        print("If you know the position of a bomb, type 'M' before the coordinates of that , e.g. MA5.")
        print("You win by complete all the sqaure without choosing a bomb.")
        print("Good luck!")
        input('Press [enter] when ready to play. ')
        
    elif choice != 'P':
        replit.clear()
        reset()

   
    b = [[0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0]]

    for n in range (0, 10):
        placeBomb(b)

    for row in range (0, 9):
        for column in range (0, 9):
            value = l(row, column, b)
            if value == '*':
                updateValues(row, column, b)

    k = [[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '], [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ']]

    printBoard(k)


    startTime = time.time()

    play(b, k, startTime)

def l(row, column, b):
    return b[row][column]

def placeBomb(b):
    row = random.randint(0, 8)
    column = random.randint(0, 8)
  
    currentRow = b[row]
    if not currentRow[column] == '*':
        currentRow[column] = '*'
    else:
        placeBomb(b)

def updateValues(rn, column, b):

    if rn-1 > -1:
        row = b[rn-1]
        
        if column-1 > -1:
            if not row[column-1] == '*':
                row[column-1] += 1

        if not row[column] == '*':
            row[column] += 1

        if 9 > column+1:
            if not row[column+1] == '*':
                row[column+1] += 1

    row = b[rn]

    if column-1 > -1:
        if not row[column-1] == '*':
            row[column-1] += 1

    if 9 > column+1:
        if not row[column+1] == '*':
            row[column+1] += 1

    if 9 > rn+1:
        row = b[rn+1]

        if column-1 > -1:
            if not row[column-1] == '*':
                row[column-1] += 1

        if not row[column] == '*':
            row[column] += 1

        if 9 > column+1:
            if not row[column+1] == '*':
                row[column+1] += 1

def zeroProcedure(row, column, k, b):

    if row-1 > -1:
        row_2 = k[row-1]
        if column-1 > -1: row_2[column-1] = l(row-1, column-1, b)
        row_2[column] = l(row-1, column, b)
        if 9 > column+1: row_2[column+1] = l(row-1, column+1, b)

    row_2 = k[row]
    if column-1 > -1: row_2[column-1] = l(row, column-1, b)
    if 9 > column+1: row_2[column+1] = l(row, column+1, b)

    if 9 > row+1:
        row_2 = k[row+1]
        if column-1 > -1: row_2[column-1] = l(row+1, column-1, b)
        row_2[column] = l(row+1, column, b)
        if 9 > column+1: row_2[column+1] = l(row+1, column+1, b)


def checkZeros(k, b, row, column):

    oldGrid = copy.deepcopy(k)
    zeroProcedure(row, column, k, b)
    if oldGrid == k:
        return
    while True:
        oldGrid = copy.deepcopy(k)
        for x in range (9):
            for y in range (9):
                if l(x, y, k) == 0:
                    zeroProcedure(x, y, k, b)
        if oldGrid == k:
            return

def marker(row, column, k):

    k[row][column] = '⚐'
    printBoard(k)

def printBoard(b):

    replit.clear()
    print('    A   B   C   D   E   F   G   H   I')
    print('  ╔═══╦═══╦═══╦═══╦═══╦═══╦═══╦═══╦═══╗')
    for row in range (0, 9):
        print(row,'║',l(row,0,b),'║',l(row,1,b),'║',l(row,2,b),'║',l(row,3,b),'║',l(row,4,b),'║',l(row,5,b),'║',l(row,6,b),'║',l(row,7,b),'║',l(row,8,b),'║')
        if not row == 8:
            print('  ╠═══╬═══╬═══╬═══╬═══╬═══╬═══╬═══╬═══╣')
    print('  ╚═══╩═══╩═══╩═══╩═══╩═══╩═══╩═══╩═══╝')

def choose(b, k, startTime):

    letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h' ,'i']
    numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8']
    while True:
        chosen = input('Choose a square (eg. A5) or place a marker (eg. MA5): ').lower()
        if len(chosen) == 3 and chosen[0] == 'm' and chosen[1] in letters and chosen[2] in numbers:
            column, row = (ord(chosen[1]))-97, int(chosen[2])
            marker(row, column, k)
            play(b, k, startTime)
            break
        elif len(chosen) == 2 and chosen[0] in letters and chosen[1] in numbers: break
        else: choose(b, k, startTime)
    return (ord(chosen[0]))-97, int(chosen[1])


def play(b, k, startTime):

    column, row = choose(b, k, startTime)
    v = l(row, column, b)
    if v == '*':
        printBoard(b)
        print('You Lose!')
        print('Time: ' + str(round(time.time() - startTime)) + 's')
        playAgain = input('Play again? (Y/N): ').lower()
        if playAgain == 'y':
            replit.clear()
            reset()
        else:
            quit()
    k[row][column] = v
    if v == 0:
        checkZeros(k, b, row, column)
    printBoard(k)
    squaresLeft = 0
    for x in range (0, 9):
        row = k[x]
        squaresLeft += row.count(' ')
        squaresLeft += row.count('⚐')
    if squaresLeft == 10:
        printBoard(b)
        print('You win!')
        print('Time: ' + str(round(time.time() - startTime)) + 's')
        playAgain = input('Play again? (Y/N): ')
        playAgain = playAgain.lower()
        if playAgain == 'y':
            replit.clear()
            reset()
        else:
            print("Game over!")
            print("Thanks for playing!")
            quit()
    play(b, k, startTime)

reset()
