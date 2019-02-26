1	Introduction

This report is complimentary to the code that is submitted in fulfilment of the assignment given in CE1003. This report is divided into 5 sections (excluding this one) – Objectives, Considerations, Modules, Conclusion & Annexure. The full code, in addition to being submitted under the filename: FER1_Ali_Ahmad_Nair_Chin.py, is also appended in Annex A for reference.

2	Objectives

This assignment involves the coding of a simple Python program that allows a User to play an extended version of Battleship with the Computer. The User is asked to place his/her Ships on the board according 3D coordinates (row, column, depth) and orientation (vertical or horizontal), followed by the Computer.

The User is then asked a set of 3D coordinates, which represents an attack with an impact that is 3 x 3 in area, on the Computer’s Ships, and the program will report if it is a hit, a miss, and if any Ships are sunk. After the User’s turn, the Computer will then randomly select a set of coordinates and the Computer reports the results. The game will end when either the User or Computer’s Ships are all sunk.

3	Considerations

The following considerations were ensured during the coding of the program:

Consideration 1 – Inputs had to be confined to a particular format, and invalid inputs have to be flagged to the User for re-entry.

Consideration 2 – Have appropriate outputs and messages displayed to inform the User on affirmative and reformative actions required.

Consideration 3 – Include appropriate breaks in-between actions or ‘modules’ (see Section 4), in order to create a smooth experience for the User.

Consideration 4 – Ensure all cases are accounted for and that instructions as dictated by the assignment, were all adhered to in the implementation of the program.

4	Modules

In this section, the Battleship+ code in further detail through the use of ‘modules’ is discussed. For purposes of this assignment, a ‘module’ is defined to be a sub-section of the code that collectively implements an objective.

4.1	Module 0

## Module 0.a
##=====================================================================
playagain = True
while playagain == True:

## Module 0.b        
##=====================================================================
    playagaininput = input('\nDo you want to play again? Enter \'Y\' for yes, or \'N\' for no: ')
    while playagaininput != 'Y' and playagaininput != 'N':        
        playagaininput = input('Do you want to play again? Enter \'Y\' for yes, or \'N\' for no: ')
    if playagaininput == 'N':
        print('Thank you for playing!')
        break

Definition(s)

playagain – Boolean variable
playagaininput – Restricted String variable in the format: ‘Y’ or ‘N’

Objective(s)

Implements the last part of the assignment, which asks if the User would like to play again after successfully completing a round with the computer. Implements error-checking to properly handle situations where the input is not of the correct nature or format.

4.2	Module 1

## Module 1
##=====================================================================
    print('New game!')
    matrixuser = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'  
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))

Definition(s)

matrixuser – 3-Dimentional Array
depth / length / width / depthrow  – Integer variable 

Objective(s)

Implements the generation of an empty 3 x 3 x 2 joined-array and loops through various coordinates in the length, width and depth values to cosmetically build out the User’s board, as depicted in the ‘Battleship+ Sample Run’ document.

4.3	Module 2

## Module 2
##=====================================================================   print('\nPlace a Submarine')
    print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
    print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')
    submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')
    formatissues = True
    positionissues = True
    while formatissues == True or positionissues == True:
        formatissues = False
        positionissues = False
        if len(''.join(submarinecoordinates.split())) != 5:
            formatissues = True
        else:
            for char in range(len(''.join(submarinecoordinates.split()))):
                if char % 2 == True:
                    if ord((''.join(submarinecoordinates.split()))[char]) != 44:
                        formatissues = True
                else:
                    if char != 4:               
                        if ord((''.join(submarinecoordinates.split()))[char]) < 49 or ord((''.join(submarinecoordinates.split()))[char]) > 56:
                            formatissues = True
                        else:
                            if char == 0:
                                if ord((''.join(submarinecoordinates.split()))[char]) > 54 and ord((''.join(submarinecoordinates.split()))[char+2]) > 54:
                                    positionissues = True                    
                    else:
                        if ord((''.join(submarinecoordinates.split()))[char]) < 48 or ord((''.join(submarinecoordinates.split()))[char]) > 49:
                            formatissues = True
        if formatissues == True:
            print('\nCoordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')
        elif positionissues == True:
            print('\nPosition is not allowed!\nPlease enter a coordinate where a valid orientation is allowed.')
            submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')        
        else:   
            submarineorientation = input('Vertically or horizontally orientate the Submarine (V, H)?: ')
            orientationissues = True
            while orientationissues == True:
                orientationissues = False
                if len(''.join(submarineorientation.split())) != 1:
                    orientationissues = True
                else:
                    if submarineorientation != 'H' and submarineorientation != 'V':
                        orientationissues = True
                    else:
                        if submarineorientation == 'H':                
                            if ord((''.join(submarinecoordinates.split()))[2]) > 54:
                                orientationissues = True
                        else:
                            if ord((''.join(submarinecoordinates.split()))[0]) > 54:
                                orientationissues = True
                if orientationissues == True:
                    print('\nOrientation is not allowed!\nPlease enter an orientation where such an orientation is allowed.')
                    submarineorientation = input('\nVertically or horizontally orientate the Submarine (V, H)?: ')
                else:
                    break

Definition(s)

submarinecoordinates – Restricted String variable in the format: ‘A,B,C’
submarineorientation – Restricted String variable in the format: ‘H’ or ‘V’
formatissues / positionissues – Boolean variable 
char – Integer variable

Objective(s)

Implements the questioning to the User on where the Submarine should be placed, and what orientation it should take. Implements error-checking to properly handle situations where, if a coordinate outside the board that is not allowed, is selected, a coordinate where both horizontal or vertical orientations that is not allowed, is selected, or a coordinate where either the horizontal or vertical orientation that is not allowed, is selected. Furthermore, there are appropriate checks to ensure that the inputs are of the correct nature and format.

4.4	Module 3

## Module 3                
##=====================================================================
    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        if int((''.join(submarinecoordinates.split()))[4]) == 0:
            matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
            if submarineorientation == 'H':
                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2]))))] = 'S'
                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])+1)))] = 'S'
            else:                matrixuser[0][(int(''.join(submarinecoordinates.split())[0]))*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])+1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
        else:
            matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
            if submarineorientation == 'H':
                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2]))))] = 'S'
                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])+1)))] = 'S'
            else:                matrixuser[1][(int(''.join(submarinecoordinates.split())[0]))*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])+1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))

Definition(s)

No new definitions

Objective(s)

Implements the positioning of the Submarine, provided all error-checks in Module 2 were passed, i.e. ‘S’ at the designated coordinate as indicated by the User and orientates it in the manner that the User has indicated, i.e. places 2 other ‘S’s in a continuous horizontal or vertical fashion.

4.5	Module 4

## Module 4            
##=====================================================================
    print('\nDone placing the User\'s Submarine.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break

Definition(s)

inputenterformatissues – Restricted String variable in the format: ‘’
inputenter – Boolean variable

Objective(s)

Implements a break to inform the User on the successful placement of his/her Submarine. Implements error-checking to properly handle situations where some string is typed upon hitting the ‘ENTER’ key. Ideally, there should not be any – a move which mimics the ‘ENTER’ keystroke.

4.6	Module 5

## Module 5        
##=====================================================================
    print('\nPlace a Destroyer')
    print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
    print('Note: Depth = 1 represents the Surface level. The Destroyer can only be placed at the Surface level.')
    destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
    formatissues = True
    positionissues = True
    while formatissues == True or positionissues == True:
        formatissues = False
        positionissues = False
        if len(''.join(destroyercoordinates.split())) != 5:
            formatissues = True
        else:
            for char in range(len(''.join(destroyercoordinates.split()))):
                if char % 2 == True:
                    if ord((''.join(destroyercoordinates.split()))[char]) != 44:
                        formatissues = True
                else:                
                    if char != 4:
                        if ord((''.join(destroyercoordinates.split()))[char]) < 49 or ord((''.join(destroyercoordinates.split()))[char]) > 56:
                            formatissues = True
                        else:
                            if char == 0:
                                if ord((''.join(destroyercoordinates.split()))[char]) > 54 and ord((''.join(destroyercoordinates.split()))[char+2]) > 54:
                                    positionissues = True
                    else:                        
                        if ord((''.join(destroyercoordinates.split()))[char]) != 49:
                            formatissues = True
                        else:
                            if ord((''.join(destroyercoordinates.split()))[char]) == ord((''.join(submarinecoordinates.split()))[char]):
                                if submarineorientation == 'H':
                                    if ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0]):
                                        if ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2]) or ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2])+1 or ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2])+2:
                                            positionissues = True
                                else:
                                    if ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2]) and abs(ord((''.join(destroyercoordinates.split()))[0]) - ord((''.join(submarinecoordinates.split()))[0])) < 3:
                                        if ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0]) or ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0])+1 or ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0])+2:
                                            positionissues = True                 
        if formatissues == True:
            print('\nCoordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
        elif positionissues == True:
            print('\nPosition is not allowed!\nPlease enter a coordinate where a valid orientation is allowed.')
            destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
        else:
            destroyerorientation = input('Vertically or horizontally orientate the Destroyer (V, H)?: ')
            orientationissues = True
            while orientationissues == True:
                orientationissues = False                      
                if len(''.join(destroyerorientation.split())) != 1:
                    orientationissues = True
                else:
                    if destroyerorientation != 'H' and destroyerorientation != 'V':
                        orientationissues = True
                    else:
                        if destroyerorientation == 'H':             
                            if ord((''.join(destroyercoordinates.split()))[2]) > 54:
                                orientationissues = True
                            else:
                                if matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2]))))] == 'S':
                                    orientationissues = True
                                elif matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])+1)))] == 'S':
                                   orientationissues = True                                    
                        else:
                            if ord((''.join(destroyercoordinates.split()))[0]) > 54:
                                orientationissues = True
                            else:
                                if matrixuser[1][(int(''.join(destroyercoordinates.split())[0]))*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] == 'S':
                                    orientationissues = True
                                elif matrixuser[1][(int(''.join(destroyercoordinates.split())[0])+1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] == 'S':
                                   orientationissues = True   
                if orientationissues == True:
                    print('Orientation is not allowed!\nPlease enter an orientation where such an orientation is allowed.')
                    destroyerorientation = input('\nVertically or horizontally orientate the Destroyer (V, H)?: ')
                else:
                    break

Definition(s)

destroyercoordinates – Restricted String variable in the format: ‘A,B,C’
destroyerorientation – Restricted String variable in the format: ‘H’ or ‘V’

Objective(s)

Implements the questioning to the User on where the Destroyer should be placed, and what orientation it should take. Implements error-checking to properly handle situations where if, a coordinate outside the board that is not allowed, is selected, a coordinate where both horizontal or vertical orientations that is not allowed, is selected, or a coordinate where either the horizontal or vertical orientation that is not allowed, is selected. Additionally, the coordinate placement of the Destroyer on the ‘Surface’-level, should not overlap with the User’s Submarine, nor should the orientation of the Destroyer, overlap with the User’s Submarine, if placed on the ‘Surface’-level. Furthermore, there are appropriate checks to ensure that the inputs are of the correct nature and format.

4.7	Module 6

## Module 6                
##=====================================================================        
    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'                    
        matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D'
        if destroyerorientation == 'H':
            matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2]))))] = 'D'
            matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])+1)))] = 'D'
        else:       matrixuser[1][(int(''.join(destroyercoordinates.split())[0]))*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D' matrixuser[1][(int(''.join(destroyercoordinates.split())[0])+1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D'
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))

Definition(s)

No new definitions.

Objective(s)

Implements the positioning of the Destroyer, provided all error-checks in Module 5 were passed, i.e. ‘D’ at the designated coordinate as indicated by the User and orientates it in the manner that the User has indicated, i.e. places 2 other ‘D’s in a continuous horizontal or vertical fashion.

4.8	Module 7

## Module 7            
##=====================================================================
print('\nDone placing the User\'s Destroyer.')
    print('Done placing the User\'s Ships.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break

Definition(s)

No new definitions.

Objective(s)

Implements a break to inform the User on the successful placement of his/her Submarine and Destroyer. Implements error-checking to properly handle situations where some string is typed upon hitting the ‘ENTER’ key. Ideally, there should not be any – a move which mimics the ‘ENTER’ keystroke.

4.9	Module 8

## Module 8        
##=====================================================================
    print('\nComputer is placing a Submarine.')
    import random
    computerorientationissues = True
    while computerorientationissues == True:
        computerorientationissues = False    
        computerdepthsubmarine = random.choice([0, 1])
        computerlengthsubmarine = random.randint(1, 8)
        computerwidthsubmarine = random.randint(1, 8)
        computersubmarineorientation = random.choice(['H', 'V'])
        if computerlengthsubmarine > 6 and computerwidthsubmarine > 6:
            computerorientationissues = True
        else:
            if computersubmarineorientation == 'H':                
                if computerwidthsubmarine > 6:
                    computerorientationissues = True
            else:
                if computerlengthsubmarine > 6:
                    computerorientationissues = True
        if computerorientationissues == False:       
            break

Definition(s)

random – Module in Python
computerorientationissues – Boolean variable
computerdepthsubmarine – Integer variable
computerlengthsubmarine – Integer variable
computerwidthsubmarine – Integer variable
computersubmarineorientation – Restricted String variable in the format: ‘H’ or ‘V’

Objective(s)

Implements the generation of random coordinates to place and orient the Computer’s Submarine. Implements error-checking to properly handle situations where if, the randomly generated coordinates and orientation that is not allowed, is selected. However, as the generation of the input is done by the Computer, appropriate checks to ensure that the inputs are of the correct nature and format, that were deemed necessary for the User, is not necessary in this instance.

4.10	Module 9

## Module 9        
##=====================================================================
    matrixcomputer = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    for depth in range(2):                  
        if computerdepthsubmarine == 0:
            matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
            if computersubmarineorientation == 'H':
                matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*computerwidthsubmarine] = 'S'
                matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine+1)] = 'S'
            else:                                                     
                matrixcomputer[0][computerlengthsubmarine*2][3+4*(computerwidthsubmarine-1)] = 'S'
                matrixcomputer[0][(computerlengthsubmarine+1)*2][3+4*(computerwidthsubmarine-1)] = 'S'           
        else:
            matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
            if computersubmarineorientation == 'H':
                matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*computerwidthsubmarine] = 'S'
                matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine+1)] = 'S'
            else:                                                     
                matrixcomputer[1][computerlengthsubmarine*2][3+4*(computerwidthsubmarine-1)] = 'S'
                matrixcomputer[1][(computerlengthsubmarine+1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
    print('Done placing the Computer\'s Submarine.')

Definition(s)

matrixcomputer – 3-Dimentional Array

Objective(s)

Implements the generation of an empty 3 x 3 x 2 joined-array and then positions the Submarine, provided all error-checks in Module 8 were passed, i.e. ‘S’ at the designated coordinate as generated by the Computer and orientates it in the manner that the Computer has generated, i.e. places 2 other ‘S’s in a continuous horizontal or vertical fashion.

4.11	Module 10

## Module 10
##=====================================================================
    print('Computer is placing a Destroyer.')
    import random
    computerorientationissues = True
    while computerorientationissues == True:
        computerorientationissues = False
        computerdepthdestroyer = 1
        computerlengthdestroyer = random.randint(1, 8)
        computerwidthdestroyer = random.randint(1, 8)
        computerdestroyerorientation = random.choice(['H', 'V'])
        if computerlengthdestroyer > 6 and computerwidthdestroyer > 6:
            computerorientationissues = True
        else:
            if computerdestroyerorientation == 'H':                
                if computerwidthdestroyer > 6:
                    computerorientationissues = True
                else:
                    if computerdepthsubmarine == 1:
                        if computerlengthdestroyer == computerlengthsubmarine or computerlengthdestroyer == computerlengthsubmarine+1 or computerlengthdestroyer == computerlengthsubmarine+2:
                            computerorientationissues = True
                        else:
                            if matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*computerwidthdestroyer] == 'S':
                                computerorientationissues = True
                            elif matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer+1)] == 'S':
                                computerorientationissues = True                                
            else:
                if computerlengthdestroyer > 6:
                    computerorientationissues = True
                else:
                    if computerdepthsubmarine == 1:
                        if computerwidthdestroyer == computerwidthsubmarine or computerwidthdestroyer == computerwidthsubmarine+1 or computerwidthdestroyer == computerwidthsubmarine+2:
                            computerorientationissues = True
                        else:
                            if matrixcomputer[1][computerlengthdestroyer*2][3+4*(computerwidthdestroyer-1)] == 'S':
                                computerorientationissues = True
                            elif matrixcomputer[1][(computerlengthdestroyer+1)*2][3+4*(computerwidthdestroyer-1)] == 'S':
                                computerorientationissues = True                     
        if computerorientationissues == False:       
            break

Definition(s)

No new definitions.

Objective(s)

Implements the generation of random coordinates to place and orient the Computer’s Destroyer. Implements error-checking to properly handle situations where if, the randomly generated coordinates and orientation that is not allowed, is selected. Additionally, the coordinate placement of the Destroyer on the ‘Surface’-level, should not overlap with the Computer’s Submarine, nor should the orientation of the Destroyer, overlap with the Computer’s Submarine, if placed on the ‘Surface’-level. However, as the generation of the input is done by the Computer, appropriate checks to ensure that the inputs are of the correct nature and format, that were deemed necessary for the User, is not necessary in this instance.

4.12	Module 11

## Module 11
##=====================================================================          
    matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer-1)] = 'D'
    if computerdestroyerorientation == 'H':
        matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*computerwidthdestroyer] = 'D'
        matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer+1)] = 'D'
    else:
        matrixcomputer[1][computerlengthdestroyer*2][3+4*(computerwidthdestroyer-1)] = 'D'
        matrixcomputer[1][(computerlengthdestroyer+1)*2][3+4*(computerwidthdestroyer-1)] = 'D'
    print('Done placing the Computer\'s Destroyer.')
    print('Done placing the Computer\'s Ships.')

Definition(s)

matrixcomputer – 3-Dimentional Array

Objective(s)

Implements the positioning of the Destroyer, provided all error-checks in Module 10 were passed, i.e. ‘D’ at the designated coordinate as generated by the Computer and orientates it in the manner that the Computer has generated, i.e. places 2 other ‘D’s in a continuous horizontal or vertical fashion.

4.13	Module 12

## Module 12    
##===================================================================== 
    showmatrixcomputerforassessment = False
    while showmatrixcomputerforassessment == False:
        showmatrixcomputerforassessment = True
        computersmatrix = input('[For Assessment Only] Show the Computer\'s ship placements? Enter \'Y\' for yes, or \'N\' for no: ')
        if computersmatrix != 'Y' and computersmatrix != 'N':
            showmatrixcomputerforassessment = False                
        if computersmatrix == 'Y' and showmatrixcomputerforassessment == True:
            print('\nThe Computer\'s Board')
            print('\n   1   2   3   4   5   6   7   8\n')
            for depth in range(2):
                if depth == 0:
                    print('Underwater')
                    for length in range(16):
                        if length % 2 == False:
                            matrixcomputer[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixcomputer[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixcomputer[depth][length][width] = '-'
                else:
                    print('Surface')
                    for length in range(16):
                        if length % 2 == False:
                            matrixcomputer[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixcomputer[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixcomputer[depth][length][width] = '-'                    
                for depthrow in matrixcomputer[depth]:
                    print(''.join(depthrow))
            break
        elif computersmatrix == 'N' and showmatrixcomputerforassessment == True:
            break

Definition(s)

showmatrixcomputerforassessment – Boolean variable
computersmatrix – Restricted String variable in the format: ‘Y’ or ‘N’

Objective(s)

Implements the questioning to the Assessor, if he/she wants to view the successful placement of the Computer’s Ships. Implements error-checking to properly handle situations where if, the input provided by the Assessor that is not allowed, is selected. 

4.14	Module 13

## Module 13        
##===================================================================== 
    print('\nReady to start the game.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break

Definition(s)

No new definitions.

Objective(s)

Implements a break to inform the User on the commencement of the game. Implements error-checking to properly handle situations where some string is typed upon hitting the ‘ENTER’ key. Ideally, there should not be any – a move which mimics the ‘ENTER’ keystroke.

4.15	Module 14

## Module 14        
##=====================================================================
    print('\nThe Computer\'s Board')
    matrixcomputerinplay = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixcomputerinplay[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixcomputerinplay[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixcomputerinplay[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixcomputerinplay[depth][length][width] = '-'
        for depthrow in matrixcomputerinplay[depth]:
            print(''.join(depthrow))

Definition(s)

matrixcomputerinplay – 3-Dimentional Array 

Objective(s)

Implements the generation of an empty 3 x 3 x 2 joined-array and loops through various coordinates in the length, width and depth values to cosmetically build out the Computer’s board that would be displayed in the game. This is different from the matrixcomputer board generated in Module X, which houses the actual positions of the Computer’s Ships. This board depicts an empty Array, which is displayed to the User, when he/she attempts to attack the Computer’s Ships.

4.16	Module 15

## Module 15.a          
##=====================================================================
    turn = 0
    usersubmarineunitshit = 0
    userdestroyerunitshit = 0
    gameends = False
    while gameends == False:
        turn = turn + 1
        if turn % 2 == 1:
            print('\nUser\'s turn')
            if turn > 1:
                inputenterformatissues = True
                while inputenterformatissues == True:
                    inputenterformatissues = False
                    inputenter = input('Hit \'ENTER\' to continue.')
                    if len(inputenter) != 0:
                        inputenterformatissues = True
                    if inputenterformatissues == False:
                        print('\nThe Computer\'s Board')
                        print('\n   1   2   3   4   5   6   7   8\n')
                        for depth in range(2):
                            if depth == 0:
                                print('Underwater')
                                for length in range(16):
                                    if length % 2 == False:
                                        matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                    for width in range(2,34):
                                        if width % 4 == True:
                                            matrixcomputerinplay[depth][length][width] = '|'
                                    for width in range(3,34):
                                        if length % 2 == True:
                                            matrixcomputerinplay[depth][length][width] = '-'
                            else:
                                print('Surface')
                                for length in range(16):
                                    if length % 2 == False:
                                        matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                    for width in range(2,34):
                                        if width % 4 == True:
                                            matrixcomputerinplay[depth][length][width] = '|'
                                    for width in range(3,34):
                                        if length % 2 == True:
                                            matrixcomputerinplay[depth][length][width] = '-'
                            for depthrow in matrixcomputerinplay[depth]:
                                print(''.join(depthrow))
                        print('\nUser\'s turn')
                        break

Definition(s)

turn / usersubmarineunitshit / userdestroyerunitshit  – Integer variable
gameends – Boolean variable
Objective(s)

Implements the start of the turn-based system of the game. The turn Integer variable uses a remainder-divisor-of-2 system to decide if it is the Computer’s turn or the User’s turn, in a WHILE loop that runs so long as the conditions that end the game (gameends Boolean variable) is FALSE. Beyond the first turn, the User needs to see the Computer’s board in order to make a decision on which coordinate to attack. Appropriate breaks using the ‘ENTER’ key also ensures that the pacing of the game is comfortable.

## Module 15.b            
##=====================================================================                 
            print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')        
            userattackcoordinates = input('Enter a coordinate to attack the Computer\'s ships: ')        
            formatissues = True
            while formatissues == True:
                formatissues = False
                if len(''.join(userattackcoordinates.split())) != 5:
                    formatissues = True
                else:
                    for char in range(len(''.join(userattackcoordinates.split()))):
                        if char % 2 == True:
                            if ord((''.join(userattackcoordinates.split()))[char]) != 44:
                                formatissues = True
                        else:
                            if char != 4:               
                                if ord((''.join(userattackcoordinates.split()))[char]) < 49 or ord((''.join(userattackcoordinates.split()))[char]) > 56:
                                    formatissues = True                    
                            else:
                                if ord((''.join(userattackcoordinates.split()))[char]) < 48 or ord((''.join(userattackcoordinates.split()))[char]) > 49:
                                    formatissues = True
                if formatissues == True:
                    print('Coordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
                    print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')        
                    userattackcoordinates = input('Enter a coordinate to attack the Computer\'s ships: ')
                else:
                    computersubmarineunitshit = 0
                    computerdestroyerunitshit = 0
                    userattacksuccess = False                    
                    for userattacklength in range(int((''.join(userattackcoordinates.split()))[0])-1, int((''.join(userattackcoordinates.split()))[0])+2):
                        for userattackwidth in range(int((''.join(userattackcoordinates.split()))[2])-1, int((''.join(userattackcoordinates.split()))[2])+2):
                            if 0 < userattacklength < 9 and 0 < userattackwidth < 9:
                                if matrixcomputer[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == 'S':
                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                        
                                    userattacksuccess = True
                                elif matrixcomputer[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == 'D':                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                        
                                    userattacksuccess = True
                                elif matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == '$':                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                                    
                                else:                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '*'                                               

Definition(s)

userattackcoordinates – Restricted String variable in the format: ‘A,B,C’
computersubmarineunitshit / computerdestroyerunitshit / userattacklength / userattackwidth – Integer variable
userattacksuccess – Boolean variable 

Objective(s)

Implements the questioning to the User on which coordinate on the Computer’s board to attack. Implements error-checking to properly handle situations where if, a coordinate outside the board that is not allowed, is selected. Once a coordinate is selected, reference is made to the matrixcomputer if the 3 x 3 area of attack contains any ‘S’ or ‘D’ units (Units, i.e. refers to a single String variable of the 3 variables that makes up a full Submarine or a Destroyer). If any of the areas of attack contains an ‘S’ or a ‘D’, then the matrixcomputerinplay displays ‘$’, if not, it displays a ‘*’, as stipulated in the ‘Battleship+ Sample Run’ document. The userattacksuccess Boolean variable and the usersubmarineunitshit / userdestroyerunitshit Integer variables from Module 15.a controls the display message and the gameends Boolean variable, i.e. the condition upon which the game ends.

## Module 15.c
##=====================================================================                 
                    print('\nThe Computer\'s Board')
                    print('\n   1   2   3   4   5   6   7   8\n')
                    for depth in range(2):
                        if depth == 0:
                            print('Underwater')
                            for length in range(16):
                                if length % 2 == False:
                                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                for width in range(2,34):
                                    if width % 4 == True:
                                        matrixcomputerinplay[depth][length][width] = '|'
                                for width in range(3,34):
                                    if length % 2 == True:
                                        matrixcomputerinplay[depth][length][width] = '-'
                        else:
                            print('Surface')
                            for length in range(16):
                                if length % 2 == False:
                                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                for width in range(2,34):
                                    if width % 4 == True:
                                        matrixcomputerinplay[depth][length][width] = '|'
                                for width in range(3,34):
                                    if length % 2 == True:
                                        matrixcomputerinplay[depth][length][width] = '-'
                        for depthrow in matrixcomputerinplay[depth]:
                            print(''.join(depthrow))            

Definition(s)

No new definitions.

Objective(s)

Implements the display of the Computer’s board (matrixcomputerinplay), post-attack, by the User.

## Module 15.d            
##===================================================================== 
                    if userattacksuccess == True:
                        print('\nHit at area centering',''.join(userattackcoordinates)+'.')
                    elif userattacksuccess == False:
                        print('\nAttack at area centering',userattackcoordinates,'is a total miss.')           
                    for length in range(1,9):
                        for width in range(1,9):
                            for depth in range(0,2):
                                if matrixcomputer[depth][(length-1)*2][3+4*(width-1)] == 'S' and matrixcomputerinplay[depth][(length-1)*2][3+4*(width-1)] == '$':
                                    computersubmarineunitshit = computersubmarineunitshit + 1
                            if matrixcomputer[1][(length-1)*2][3+4*(width-1)] == 'D' and matrixcomputerinplay[1][(length-1)*2][3+4*(width-1)] == '$':
                                computerdestroyerunitshit = computerdestroyerunitshit + 1
                    if int(computersubmarineunitshit) > 2:
                        print('Computer\'s Submarine has been sunk.')
                    if int(computerdestroyerunitshit) > 2:
                        print('Computer\'s Destroyer has been sunk.')
                    if int(computersubmarineunitshit+computerdestroyerunitshit) > 5:
                        print('Computer has lost both its ships.')
                        print('\nUser has won!')
                        gameends = True                    
        else:

Definition(s)

No new definitions.

Objective(s)

Implements the stocktake and the display of relevant messages, post-attack, to the User. If the User had selected a coordinate whose 3 x 3 area of attack contained any ‘S’ or ‘D’, then a message is displayed on the successful attack centering that coordinate. If more than 2 units i.e. computersubmarineunitshit / computerdestroyerunitshit, were destroyed, then it would be considered that the Computer had lost those ships. If the combined number of Units of both Computer’s Submarine and Destroyer exceeds 5, then it would be considered that the Computer had lost both ships and thus the gameends Boolean variable would be set to TRUE, signalling to the inner WHILE loop to break and display Module 0.

## Module 15.e            
##===================================================================== 
            print('\nComputer\'s turn')
            inputenterformatissues = True
            while inputenterformatissues == True:
                inputenterformatissues = False
                inputenter = input('Hit \'ENTER\' to continue.')
                if len(inputenter) != 0:
                    inputenterformatissues = True
                if inputenterformatissues == False:
                    break            
            computerattacklength = random.randint(1, 8)
            computerattackwidth = random.randint(1, 8)
            computerattackdepth = random.choice([0, 1])        
            computerattacksuccess = False
            for computerattacklengthcoordinate in range(computerattacklength-1, computerattacklength+2):
                for computerattackwidthcoordinate in range(computerattackwidth-1, computerattackwidth+2):
                    if 0 < computerattacklengthcoordinate < 9 and 0 < computerattackwidthcoordinate < 9:                    
                        if matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == 'S':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'
                            computerattacksuccess = True
                            usersubmarineunitshit = usersubmarineunitshit + 1
                        elif matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == 'D':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'                        
                            computerattacksuccess = True
                            userdestroyerunitshit = userdestroyerunitshit + 1                        
                        elif matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == '$':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'                            
                        else:
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '*'

Definition(s)

computerattacklength / computerattackwidth / computerattackdepth / computerattacklengthcoordinate / computerattackwidthcoordinate – Integer variable

computerattacksuccess – Boolean variable

Objective(s)

Implements the start of the turn-based system of the game. The turn Integer variable uses a remainder-divisor-of-2 system to decide if it is the Computer’s turn or the User’s turn, in a WHILE loop that runs so long as the conditions that end the game (gameends Boolean variable) is FALSE. If the remainder is 0 i.e. the turn is even, the Computer would pick a coordinate to attack. Once a coordinate is randomly selected, reference is made to the matrixuser if the 3 x 3 area of attack contains any ‘S’ or ‘D’ units (Units, i.e. refers to a single String variable of the 3 variables that makes up a full Submarine or a Destroyer). If any of the areas of attack contains an ‘S’ or a ‘D’, then the matrixuser displays ‘$’, if not, it displays a ‘*’, as stipulated in the ‘Battleship+ Sample Run’ document. Appropriate breaks using the ‘ENTER’ key also ensures that the pacing of the game is comfortable.

## Module 15.f            
##=====================================================================               
            print('\nThe User\'s Board')
            print('\n   1   2   3   4   5   6   7   8\n')
            for depth in range(2):
                if depth == 0:
                    print('Underwater')
                    for length in range(16):
                        if length % 2 == False:
                            matrixuser[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixuser[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixuser[depth][length][width] = '-'
                else:
                    print('Surface')
                    for length in range(16):
                        if length % 2 == False:
                            matrixuser[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixuser[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixuser[depth][length][width] = '-'
                for depthrow in matrixuser[depth]:
                    print(''.join(depthrow))              

Definition(s)

No new definitions.

Objective(s)

Implements the display of the User’s board (matrixuser), post-attack, by the Computer.

## Module 15.g            
##=====================================================================            
            if computerattacksuccess == True:
                print('\nHit at area centering',str(computerattacklength)+','+str(computerattackwidth)+','+str(computerattackdepth)+'.')
            elif computerattacksuccess == False:
                print('\nAttack at area centering',str(computerattacklength)+','+str(computerattackwidth)+','+str(computerattackdepth),'is a total miss.')
            if int(usersubmarineunitshit) > 2:
                print('User\'s Submarine has been sunk.')
            if int(userdestroyerunitshit) > 2:
                print('User\'s Destroyer has been sunk.')
            if int(usersubmarineunitshit+userdestroyerunitshit) > 5:
                print('User has lost both its ships.')
                print('\nComputer has won!')
                gameends = True                              
        if gameends == True:
            break

Definition(s)

No new definitions.

Objective(s)

Implements the stocktake and the display of relevant messages, post-attack, to the User. If the Computer had selected a coordinate whose 3 x 3 area of attack contained any ‘S’ or ‘D’, then a message is displayed on the successful attack centering that coordinate. If more than 2 units i.e. usersubmarineunitshit / userdestroyerunitshit, were destroyed, then it would be considered that the User had lost those ships. If the combined number of Units of both User’s Submarine and Destroyer exceeds 5, then it would be considered that the User had lost both ships and thus the gameends Boolean variable would be set to TRUE, signalling to the inner WHILE loop to break and display Module 0.

5	Limitations

Through various runs and tests, it was found that the program is robust and resilient to invalid and incorrect inputs, which is deemed successful within the context of the assignment. However, this does not mean that it is devoid of limitations as a game. While trying to build an interface that mimics the example given in the documentation, it was found that, it was not possible for the player to end the game mid-play, or save the game for future play – while not within the scope of the assignment, these are features that are common to most, if not the most basic of game, however, due to time constraints, it was not possible to code such a module.

There is also a curious circumvention that had to be done with how the User’s Destroyer could be placed after the placement of the User’s Submarine. It was discovered that, following the placement of both the User’s Ships, there were no options to proceed backwards and change the coordinate or the placement of the Ships. In hindsight, this could have been a valuable value-add for this program, however, once again due to time constraints, it was not possible to code such a module. 

The learnings from both limitations were taken note of, and future versions would include these improvements, as they would provide valuable learning experiences from a technical and a UI/UX standpoint.

6	Conclusion

In conclusion, the objectives of the assignment as laid out in Section 2 has been met and the program has been deemed stable and robust for use. Appropriate considerations and breaks has been added to increase the pace and readability of the game for the User. An additional module has been added, to display the Computer’s placement for assessment purposes. Limitations has been identified with suggestions made for future improvements. All-in-all, it has been a fun and important learning experience, with a programming language that is noted by many to be one of the most important languages to learn in the 21st century.

End of Report

 
7	Annexure

Annex A
Full Code

## Module 0.a
##======================================================================
playagain = True
while playagain == True:
## Module 1
##======================================================================
    print('New game!')
    matrixuser = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'  
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))
## Module 2
##======================================================================    print('\nPlace a Submarine')
    print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
    print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')
    submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')
    formatissues = True
    positionissues = True
    while formatissues == True or positionissues == True:
        formatissues = False
        positionissues = False
        if len(''.join(submarinecoordinates.split())) != 5:
            formatissues = True
        else:
            for char in range(len(''.join(submarinecoordinates.split()))):
                if char % 2 == True:
                    if ord((''.join(submarinecoordinates.split()))[char]) != 44:
                        formatissues = True
                else:
                    if char != 4:               
                        if ord((''.join(submarinecoordinates.split()))[char]) < 49 or ord((''.join(submarinecoordinates.split()))[char]) > 56:
                            formatissues = True
                        else:
                            if char == 0:
                                if ord((''.join(submarinecoordinates.split()))[char]) > 54 and ord((''.join(submarinecoordinates.split()))[char+2]) > 54:
                                    positionissues = True                    
                    else:
                        if ord((''.join(submarinecoordinates.split()))[char]) < 48 or ord((''.join(submarinecoordinates.split()))[char]) > 49:
                            formatissues = True
        if formatissues == True:
            print('\nCoordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')
        elif positionissues == True:
            print('\nPosition is not allowed!\nPlease enter a coordinate where a valid orientation is allowed.')
            submarinecoordinates = input('\nEnter the coordinates to place the Submarine: ')        
        else:   
            submarineorientation = input('Vertically or horizontally orientate the Submarine (V, H)?: ')
            orientationissues = True
            while orientationissues == True:
                orientationissues = False
                if len(''.join(submarineorientation.split())) != 1:
                    orientationissues = True
                else:
                    if submarineorientation != 'H' and submarineorientation != 'V':
                        orientationissues = True
                    else:
                        if submarineorientation == 'H':                
                            if ord((''.join(submarinecoordinates.split()))[2]) > 54:
                                orientationissues = True
                        else:
                            if ord((''.join(submarinecoordinates.split()))[0]) > 54:
                                orientationissues = True
                if orientationissues == True:
                    print('\nOrientation is not allowed!\nPlease enter an orientation where such an orientation is allowed.')
                    submarineorientation = input('\nVertically or horizontally orientate the Submarine (V, H)?: ')
                else:
                    break
## Module 3                
##======================================================================
    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        if int((''.join(submarinecoordinates.split()))[4]) == 0:
            matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
            if submarineorientation == 'H':
                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2]))))] = 'S'
                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])+1)))] = 'S'
            else:                matrixuser[0][(int(''.join(submarinecoordinates.split())[0]))*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'                matrixuser[0][(int(''.join(submarinecoordinates.split())[0])+1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
        else:
            matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
            if submarineorientation == 'H':
                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2]))))] = 'S'
                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])-1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])+1)))] = 'S'
            else:                matrixuser[1][(int(''.join(submarinecoordinates.split())[0]))*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'                matrixuser[1][(int(''.join(submarinecoordinates.split())[0])+1)*2][3+4*(((int(''.join(submarinecoordinates.split())[2])-1)))] = 'S'
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))
## Module 4            
##======================================================================
    print('\nDone placing the User\'s Submarine.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break
## Module 5        
##======================================================================    print('\nPlace a Destroyer')
    print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
    print('Note: Depth = 1 represents the Surface level. The Destroyer can only be placed at the Surface level.')
    destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
    formatissues = True
    positionissues = True
    while formatissues == True or positionissues == True:
        formatissues = False
        positionissues = False
        if len(''.join(destroyercoordinates.split())) != 5:
            formatissues = True
        else:
            for char in range(len(''.join(destroyercoordinates.split()))):
                if char % 2 == True:
                    if ord((''.join(destroyercoordinates.split()))[char]) != 44:
                        formatissues = True
                else:                
                    if char != 4:
                        if ord((''.join(destroyercoordinates.split()))[char]) < 49 or ord((''.join(destroyercoordinates.split()))[char]) > 56:
                            formatissues = True
                        else:
                            if char == 0:
                                if ord((''.join(destroyercoordinates.split()))[char]) > 54 and ord((''.join(destroyercoordinates.split()))[char+2]) > 54:
                                    positionissues = True
                    else:                        
                        if ord((''.join(destroyercoordinates.split()))[char]) != 49:
                            formatissues = True
                        else:
                            if ord((''.join(destroyercoordinates.split()))[char]) == ord((''.join(submarinecoordinates.split()))[char]):
                                if submarineorientation == 'H':
                                    if ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0]):
                                        if ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2]) or ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2])+1 or ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2])+2:
                                            positionissues = True
                                else:
                                    if ord((''.join(destroyercoordinates.split()))[2]) == ord((''.join(submarinecoordinates.split()))[2]) and abs(ord((''.join(destroyercoordinates.split()))[0]) - ord((''.join(submarinecoordinates.split()))[0])) < 3:
                                        if ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0]) or ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0])+1 or ord((''.join(destroyercoordinates.split()))[0]) == ord((''.join(submarinecoordinates.split()))[0])+2:
                                            positionissues = True                 
        if formatissues == True:
            print('\nCoordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
        elif positionissues == True:
            print('\nPosition is not allowed!\nPlease enter a coordinate where a valid orientation is allowed.')
            destroyercoordinates = input('\nEnter the coordinates to place the Destroyer: ')
        else:
            destroyerorientation = input('Vertically or horizontally orientate the Destroyer (V, H)?: ')
            orientationissues = True
            while orientationissues == True:
                orientationissues = False                      
                if len(''.join(destroyerorientation.split())) != 1:
                    orientationissues = True
                else:
                    if destroyerorientation != 'H' and destroyerorientation != 'V':
                        orientationissues = True
                    else:
                        if destroyerorientation == 'H':             
                            if ord((''.join(destroyercoordinates.split()))[2]) > 54:
                                orientationissues = True
                            else:
                                if matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2]))))] == 'S':
                                    orientationissues = True
                                elif matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])+1)))] == 'S':
                                   orientationissues = True                                    
                        else:
                            if ord((''.join(destroyercoordinates.split()))[0]) > 54:
                                orientationissues = True
                            else:
                                if matrixuser[1][(int(''.join(destroyercoordinates.split())[0]))*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] == 'S':
                                    orientationissues = True
                                elif matrixuser[1][(int(''.join(destroyercoordinates.split())[0])+1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] == 'S':
                                   orientationissues = True   
                if orientationissues == True:
                    print('Orientation is not allowed!\nPlease enter an orientation where such an orientation is allowed.')
                    destroyerorientation = input('\nVertically or horizontally orientate the Destroyer (V, H)?: ')
                else:
                    break
## Module 6                
##======================================================================    print('\nThe User\'s Board')
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixuser[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixuser[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixuser[depth][length][width] = '-'                    
        matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D'
        if destroyerorientation == 'H':
            matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2]))))] = 'D'
            matrixuser[1][(int(''.join(destroyercoordinates.split())[0])-1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])+1)))] = 'D'
        else:            matrixuser[1][(int(''.join(destroyercoordinates.split())[0]))*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D'            matrixuser[1][(int(''.join(destroyercoordinates.split())[0])+1)*2][3+4*(((int(''.join(destroyercoordinates.split())[2])-1)))] = 'D'
        for depthrow in matrixuser[depth]:
            print(''.join(depthrow))
## Module 7            
##======================================================================    print('\nDone placing the User\'s Destroyer.')
    print('Done placing the User\'s Ships.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break
## Module 8        
##======================================================================
    print('\nComputer is placing a Submarine.')
    import random
    computerorientationissues = True
    while computerorientationissues == True:
        computerorientationissues = False    
        computerdepthsubmarine = random.choice([0, 1])
        computerlengthsubmarine = random.randint(1, 8)
        computerwidthsubmarine = random.randint(1, 8)
        computersubmarineorientation = random.choice(['H', 'V'])
        if computerlengthsubmarine > 6 and computerwidthsubmarine > 6:
            computerorientationissues = True
        else:
            if computersubmarineorientation == 'H':                
                if computerwidthsubmarine > 6:
                    computerorientationissues = True
            else:
                if computerlengthsubmarine > 6:
                    computerorientationissues = True
        if computerorientationissues == False:       
            break
## Module 9        
##====================================================================== 
    matrixcomputer = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    for depth in range(2):                  
        if computerdepthsubmarine == 0:
            matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
            if computersubmarineorientation == 'H':
                matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*computerwidthsubmarine] = 'S'
                matrixcomputer[0][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine+1)] = 'S'
            else:                                                     
                matrixcomputer[0][computerlengthsubmarine*2][3+4*(computerwidthsubmarine-1)] = 'S'
                matrixcomputer[0][(computerlengthsubmarine+1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
            
        else:
            matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
            if computersubmarineorientation == 'H':
                matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*computerwidthsubmarine] = 'S'
                matrixcomputer[1][(computerlengthsubmarine-1)*2][3+4*(computerwidthsubmarine+1)] = 'S'
            else:                                                     
                matrixcomputer[1][computerlengthsubmarine*2][3+4*(computerwidthsubmarine-1)] = 'S'
                matrixcomputer[1][(computerlengthsubmarine+1)*2][3+4*(computerwidthsubmarine-1)] = 'S'
    print('Done placing the Computer\'s Submarine.')
## Module 10
##======================================================================  
    print('Computer is placing a Destroyer.')
    import random
    computerorientationissues = True
    while computerorientationissues == True:
        computerorientationissues = False
        computerdepthdestroyer = 1
        computerlengthdestroyer = random.randint(1, 8)
        computerwidthdestroyer = random.randint(1, 8)
        computerdestroyerorientation = random.choice(['H', 'V'])
        if computerlengthdestroyer > 6 and computerwidthdestroyer > 6:
            computerorientationissues = True
        else:
            if computerdestroyerorientation == 'H':                
                if computerwidthdestroyer > 6:
                    computerorientationissues = True
                else:
                    if computerdepthsubmarine == 1:
                        if computerlengthdestroyer == computerlengthsubmarine or computerlengthdestroyer == computerlengthsubmarine+1 or computerlengthdestroyer == computerlengthsubmarine+2:
                            computerorientationissues = True
                        else:
                            if matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*computerwidthdestroyer] == 'S':
                                computerorientationissues = True
                            elif matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer+1)] == 'S':
                                computerorientationissues = True                                
            else:
                if computerlengthdestroyer > 6:
                    computerorientationissues = True
                else:
                    if computerdepthsubmarine == 1:
                        if computerwidthdestroyer == computerwidthsubmarine or computerwidthdestroyer == computerwidthsubmarine+1 or computerwidthdestroyer == computerwidthsubmarine+2:
                            computerorientationissues = True
                        else:
                            if matrixcomputer[1][computerlengthdestroyer*2][3+4*(computerwidthdestroyer-1)] == 'S':
                                computerorientationissues = True
                            elif matrixcomputer[1][(computerlengthdestroyer+1)*2][3+4*(computerwidthdestroyer-1)] == 'S':
                                computerorientationissues = True                     
        if computerorientationissues == False:       
            break
## Module 11
##====================================================================== 
    matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer-1)] = 'D'
    if computerdestroyerorientation == 'H':
        matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*computerwidthdestroyer] = 'D'
        matrixcomputer[1][(computerlengthdestroyer-1)*2][3+4*(computerwidthdestroyer+1)] = 'D'
    else:
        matrixcomputer[1][computerlengthdestroyer*2][3+4*(computerwidthdestroyer-1)] = 'D'
        matrixcomputer[1][(computerlengthdestroyer+1)*2][3+4*(computerwidthdestroyer-1)] = 'D'
    print('Done placing the Computer\'s Destroyer.')
    print('Done placing the Computer\'s Ships.')
## Module 12    
##======================================================================               
    showmatrixcomputerforassessment = False
    while showmatrixcomputerforassessment == False:
        showmatrixcomputerforassessment = True
        computersmatrix = input('[For Assessment Only] Show the Computer\'s ship placements? Enter \'Y\' for yes, or \'N\' for no: ')
        if computersmatrix != 'Y' and computersmatrix != 'N':
            showmatrixcomputerforassessment = False                
        if computersmatrix == 'Y' and showmatrixcomputerforassessment == True:
            print('\nThe Computer\'s Board')
            print('\n   1   2   3   4   5   6   7   8\n')
            for depth in range(2):
                if depth == 0:
                    print('Underwater')
                    for length in range(16):
                        if length % 2 == False:
                            matrixcomputer[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixcomputer[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixcomputer[depth][length][width] = '-'
                else:
                    print('Surface')
                    for length in range(16):
                        if length % 2 == False:
                            matrixcomputer[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixcomputer[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixcomputer[depth][length][width] = '-'                    
                for depthrow in matrixcomputer[depth]:
                    print(''.join(depthrow))
            break
        elif computersmatrix == 'N' and showmatrixcomputerforassessment == True:
            break
## Module 13        
##======================================================================                
    print('\nReady to start the game.')
    inputenterformatissues = True
    while inputenterformatissues == True:
        inputenterformatissues = False
        inputenter = input('Hit \'ENTER\' to continue.')
        if len(inputenter) != 0:
            inputenterformatissues = True
        if inputenterformatissues == False:
            break
## Module 14        
##======================================================================
    print('\nThe Computer\'s Board')
    matrixcomputerinplay = [[[' ' for length in range(34)] for width in range(16)] for depth in range(2)]
    print('\n   1   2   3   4   5   6   7   8\n')
    for depth in range(2):
        if depth == 0:
            print('Underwater')
            for length in range(16):
                if length % 2 == False:
                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixcomputerinplay[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixcomputerinplay[depth][length][width] = '-'
        else:
            print('Surface')
            for length in range(16):
                if length % 2 == False:
                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                for width in range(2,34):
                    if width % 4 == True:
                        matrixcomputerinplay[depth][length][width] = '|'
                for width in range(3,34):
                    if length % 2 == True:
                        matrixcomputerinplay[depth][length][width] = '-'
        for depthrow in matrixcomputerinplay[depth]:
            print(''.join(depthrow))
## Module 15.a          
##======================================================================
    turn = 0
    usersubmarineunitshit = 0
    userdestroyerunitshit = 0
    gameends = False
    while gameends == False:
        turn = turn + 1
        if turn % 2 == 1:
            print('\nUser\'s turn')
            if turn > 1:
                inputenterformatissues = True
                while inputenterformatissues == True:
                    inputenterformatissues = False
                    inputenter = input('Hit \'ENTER\' to continue.')
                    if len(inputenter) != 0:
                        inputenterformatissues = True
                    if inputenterformatissues == False:
                        print('\nThe Computer\'s Board')
                        print('\n   1   2   3   4   5   6   7   8\n')
                        for depth in range(2):
                            if depth == 0:
                                print('Underwater')
                                for length in range(16):
                                    if length % 2 == False:
                                        matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                    for width in range(2,34):
                                        if width % 4 == True:
                                            matrixcomputerinplay[depth][length][width] = '|'
                                    for width in range(3,34):
                                        if length % 2 == True:
                                            matrixcomputerinplay[depth][length][width] = '-'
                            else:
                                print('Surface')
                                for length in range(16):
                                    if length % 2 == False:
                                        matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                    for width in range(2,34):
                                        if width % 4 == True:
                                            matrixcomputerinplay[depth][length][width] = '|'
                                    for width in range(3,34):
                                        if length % 2 == True:
                                            matrixcomputerinplay[depth][length][width] = '-'
                            for depthrow in matrixcomputerinplay[depth]:
                                print(''.join(depthrow))
                        print('\nUser\'s turn')
                        break
## Module 15.b            
##======================================================================                 
            print('\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
            print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')        
            userattackcoordinates = input('Enter a coordinate to attack the Computer\'s ships: ')        
            formatissues = True
            while formatissues == True:
                formatissues = False
                if len(''.join(userattackcoordinates.split())) != 5:
                    formatissues = True
                else:
                    for char in range(len(''.join(userattackcoordinates.split()))):
                        if char % 2 == True:
                            if ord((''.join(userattackcoordinates.split()))[char]) != 44:
                                formatissues = True
                        else:
                            if char != 4:               
                                if ord((''.join(userattackcoordinates.split()))[char]) < 49 or ord((''.join(userattackcoordinates.split()))[char]) > 56:
                                    formatissues = True                    
                            else:
                                if ord((''.join(userattackcoordinates.split()))[char]) < 48 or ord((''.join(userattackcoordinates.split()))[char]) > 49:
                                    formatissues = True
                if formatissues == True:
                    print('Coordinates format incorrect!\nPlease enter the coordinates following this format: 3,4,1 where Length = 3, Width = 4, and Depth = 1.')
                    print('Note: Depth = 0 represents the Underwater layer, while Depth = 1 represents the Surface level.')        
                    userattackcoordinates = input('Enter a coordinate to attack the Computer\'s ships: ')
                else:
                    computersubmarineunitshit = 0
                    computerdestroyerunitshit = 0
                    userattacksuccess = False                    
                    for userattacklength in range(int((''.join(userattackcoordinates.split()))[0])-1, int((''.join(userattackcoordinates.split()))[0])+2):
                        for userattackwidth in range(int((''.join(userattackcoordinates.split()))[2])-1, int((''.join(userattackcoordinates.split()))[2])+2):
                            if 0 < userattacklength < 9 and 0 < userattackwidth < 9:
                                if matrixcomputer[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == 'S':
                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                        
                                    userattacksuccess = True
                                elif matrixcomputer[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == 'D':
                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                        
                                    userattacksuccess = True
                                elif matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] == '$':                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '$'                                    
                                else:                                    matrixcomputerinplay[int((''.join(userattackcoordinates.split()))[4])][(userattacklength-1)*2][3+4*(userattackwidth-1)] = '*'                                               
## Module 15.c
##======================================================================                 
                    print('\nThe Computer\'s Board')
                    print('\n   1   2   3   4   5   6   7   8\n')
                    for depth in range(2):
                        if depth == 0:
                            print('Underwater')
                            for length in range(16):
                                if length % 2 == False:
                                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                for width in range(2,34):
                                    if width % 4 == True:
                                        matrixcomputerinplay[depth][length][width] = '|'
                                for width in range(3,34):
                                    if length % 2 == True:
                                        matrixcomputerinplay[depth][length][width] = '-'
                        else:
                            print('Surface')
                            for length in range(16):
                                if length % 2 == False:
                                    matrixcomputerinplay[depth][length][0] = str(int(length/2)+1)
                                for width in range(2,34):
                                    if width % 4 == True:
                                        matrixcomputerinplay[depth][length][width] = '|'
                                for width in range(3,34):
                                    if length % 2 == True:
                                        matrixcomputerinplay[depth][length][width] = '-'
                        for depthrow in matrixcomputerinplay[depth]:
                            print(''.join(depthrow))            
## Module 15.d            
##======================================================================               
                    if userattacksuccess == True:
                        print('\nHit at area centering',''.join(userattackcoordinates)+'.')
                    elif userattacksuccess == False:
                        print('\nAttack at area centering',userattackcoordinates,'is a total miss.')           
                    for length in range(1,9):
                        for width in range(1,9):
                            for depth in range(0,2):
                                if matrixcomputer[depth][(length-1)*2][3+4*(width-1)] == 'S' and matrixcomputerinplay[depth][(length-1)*2][3+4*(width-1)] == '$':
                                    computersubmarineunitshit = computersubmarineunitshit + 1
                            if matrixcomputer[1][(length-1)*2][3+4*(width-1)] == 'D' and matrixcomputerinplay[1][(length-1)*2][3+4*(width-1)] == '$':
                                computerdestroyerunitshit = computerdestroyerunitshit + 1
                    if int(computersubmarineunitshit) > 2:
                        print('Computer\'s Submarine has been sunk.')
                    if int(computerdestroyerunitshit) > 2:
                        print('Computer\'s Destroyer has been sunk.')
                    if int(computersubmarineunitshit+computerdestroyerunitshit) > 5:
                        print('Computer has lost both its ships.')
                        print('\nUser has won!')
                        gameends = True                    
        else:
## Module 15.e            
##======================================================================                         
            print('\nComputer\'s turn')
            inputenterformatissues = True
            while inputenterformatissues == True:
                inputenterformatissues = False
                inputenter = input('Hit \'ENTER\' to continue.')
                if len(inputenter) != 0:
                    inputenterformatissues = True
                if inputenterformatissues == False:
                    break            
            computerattacklength = random.randint(1, 8)
            computerattackwidth = random.randint(1, 8)
            computerattackdepth = random.choice([0, 1])        
            computerattacksuccess = False
            for computerattacklengthcoordinate in range(computerattacklength-1, computerattacklength+2):
                for computerattackwidthcoordinate in range(computerattackwidth-1, computerattackwidth+2):
                    if 0 < computerattacklengthcoordinate < 9 and 0 < computerattackwidthcoordinate < 9:                    
                        if matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == 'S':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'
                            computerattacksuccess = True
                            usersubmarineunitshit = usersubmarineunitshit + 1
                        elif matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == 'D':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'                        
                            computerattacksuccess = True
                            userdestroyerunitshit = userdestroyerunitshit + 1                        
                        elif matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] == '$':
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '$'                            
                        else:
                            matrixuser[computerattackdepth][(computerattacklengthcoordinate-1)*2][3+4*(computerattackwidthcoordinate-1)] = '*'
## Module 15.f            
##======================================================================                 
            print('\nThe User\'s Board')
            print('\n   1   2   3   4   5   6   7   8\n')
            for depth in range(2):
                if depth == 0:
                    print('Underwater')
                    for length in range(16):
                        if length % 2 == False:
                            matrixuser[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixuser[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixuser[depth][length][width] = '-'
                else:
                    print('Surface')
                    for length in range(16):
                        if length % 2 == False:
                            matrixuser[depth][length][0] = str(int(length/2)+1)
                        for width in range(2,34):
                            if width % 4 == True:
                                matrixuser[depth][length][width] = '|'
                        for width in range(3,34):
                            if length % 2 == True:
                                matrixuser[depth][length][width] = '-'
                for depthrow in matrixuser[depth]:
                    print(''.join(depthrow))              
## Module 15.g            
##======================================================================                
            if computerattacksuccess == True:
                print('\nHit at area centering',str(computerattacklength)+','+str(computerattackwidth)+','+str(computerattackdepth)+'.')
            elif computerattacksuccess == False:
                print('\nAttack at area centering',str(computerattacklength)+','+str(computerattackwidth)+','+str(computerattackdepth),'is a total miss.')
            if int(usersubmarineunitshit) > 2:
                print('User\'s Submarine has been sunk.')
            if int(userdestroyerunitshit) > 2:
                print('User\'s Destroyer has been sunk.')
            if int(usersubmarineunitshit+userdestroyerunitshit) > 5:
                print('User has lost both its ships.')
                print('\nComputer has won!')
                gameends = True                              
        if gameends == True:
            break
## Module 0.b        
##======================================================================
    playagaininput = input('\nDo you want to play again? Enter \'Y\' for yes, or \'N\' for no: ')
    while playagaininput != 'Y' and playagaininput != 'N':        
        playagaininput = input('Do you want to play again? Enter \'Y\' for yes, or \'N\' for no: ')
    if playagaininput == 'N':
        print('Thank you for playing!')
        break

End of Annexure
