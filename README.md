# pygame

# 한경대 2020810072 컴퓨터 응용 수학부 진영호
import pygame
import sys
import random
from time import sleep

bookCount = int()
gameCount = int()
computerCount = int()


width = 480
height = 640
mygameImage = ['game1.png','game2.png','game3.png']
mybookImage = ['book1.png','book2.png','book3.png']
mycomputerImage = ['computer1.png','computer2.png','computer3.png']

def writeScore(count1, count2, count3):
    global gamePad, grade
    grade = int(bookCount*2 + gameCount*-2)
    font = pygame.font.Font('NanumGothic.ttf', 20)
    text1 = font.render('획득한 책의 수 :' + str(count1), True, (255,255,255))
    text2 = font.render('획득한 게임의 수 :' + str(count2), True, (255,255,255))
    text3 = font.render('획득한 컴퓨터의 수 :' + str(count3), True, (255,255,255))
    text4 = font.render('예상 점수 :' + str(50 + grade), True, (255,255,255))
    text5 = font.render('먹은 아이템 수:' + str(eat), True, (255,255,255))
    gamePad.blit(text1, (10, 0))
    gamePad.blit(text2, (10, 20))
    gamePad.blit(text3, (10, 40))
    gamePad.blit(text4, (300, 0))
    gamePad.blit(text5, (300, 20))
    
             
def writeMessage(text):
    global gamePad
    textfont = pygame.font.Font('NanumGothic.ttf', 80)
    text = textfont.render(text, True, (255,0,0))
    textpos = text.get_rect()
    textpos.center = (width/2, height/2)
    gamePad.blit(text, textpos)
    pygame.display.update()
    pygame.mixer.music.stop()
    sleep(3)
    
def ending0():
    global gamePad
    writeMessage("프로게이머")
def ending1():
    global gamePad
    writeMessage("학점 : F")
def ending2():
    global gamePad
    writeMessage("학점 : D")
def ending3():
    global gamePad
    writeMessage("학점 : C")
def ending4():
    global gamePad
    writeMessage("학점 : B")
def ending5():
    global gamePad
    writeMessage("학점 : A")
def ending6():
    global gamePad
    writeMessage("학점 : A+")

def drawObject(obj, x, y):
    global gamePad
    gamePad.blit(obj, (x, y))

def initGame():
    global gamePad, clock, background, me, bookCount, gameCount, computerCount, light, grade, totalCount, eat, touchSound
    eat = int()
    totalCount = int()
    pygame.init()
    gamePad = pygame.display.set_mode((width, height))
    pygame.display.set_caption("GiMalGame")
    background = pygame.image.load("baegyeong.png")
    me = pygame.image.load("me1.png")
    light = pygame.image.load("light1.png")
    touchSound = pygame.mixer.Sound('mario.wav')
    pygame.mixer.music.load('Helltaker.wav')
    pygame.mixer.music.play(-1)
    clock = pygame.time.Clock()

def runGame():
    
    global gamePad, clock, background, me, bookCount, gameCount, computerCount, light, grade, totalCount, eat, touchSound
    meSize = me.get_rect().size
    meWidth  = meSize[0]
    meHeight = meSize[1]

    x = int(width * 0.45)
    y = int(height * 0.9)
    meX = 0

    mygame = pygame.image.load(random.choice(mygameImage))
    mygameSize = mygame.get_rect().size
    mygameWidth = mygameSize[0]
    mygameHeight = mygameSize[1]
    mygameX = random.randrange(0, width - mygameWidth)
    mygameY = 0
    mygameSpeed = 3

    mybook = pygame.image.load(random.choice(mybookImage))
    mybookSize = mybook.get_rect().size
    mybookWidth = mybookSize[0]
    mybookHeight = mybookSize[1]
    mybookX = random.randrange(0, width - mybookWidth)
    mybookY = 0
    mybookSpeed = 3

    mycomputer = pygame.image.load(random.choice(mycomputerImage))
    mycomputerSize = mycomputer.get_rect().size
    mycomputerWidth = mycomputerSize[0]
    mycomputerHeight = mycomputerSize[1]
    mycomputerX = random.randrange(0, width - mycomputerWidth)
    mycomputerY = 0
    mycomputerSpeed = 3
    
    bookTouch = False
    gameTouch = False
    computerTouch = False
    
    onGame = False
    while not onGame:
        for event in pygame.event.get():
            if event.type in [pygame.QUIT]:
                pygame.quit()
                sys.exit()

            if event.type in [pygame.KEYDOWN]:
                if event.key == pygame.K_LEFT:
                    meX -= 5

                elif event.key == pygame.K_RIGHT:
                    meX += 5

            if event.type in [pygame.KEYUP]:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    meX = 0
                    
        drawObject(background, 0, 0)

        x += meX
        if x < 0:
            x = 0

        elif x > width - meWidth:
            x = width - meWidth

        drawObject(me, x, y)

        if y < mygameY + mygameHeight:
            if (mygameX > x and mygameX < x + meX) or \
                (mygameX + mygameWidth > x and mygameX + mygameWidth < x + meWidth):
                touchSound.play()
                gameTouch = True
                eat +=1
                totalCount +=1
                gameCount +=1
                
        if y < mybookY + mybookHeight:
            if (mybookX > x and mybookX < x + meX) or \
                (mybookX + mybookWidth > x and mybookX + mybookWidth < x + meWidth):
                touchSound.play()
                bookTouch = True
                eat +=1
                totalCount +=1
                bookCount +=1
                
        if y < mycomputerY + mycomputerHeight:
            if (mycomputerX > x and mycomputerX < x + meX) or \
                (mycomputerX + mycomputerWidth > x and mycomputerX + mycomputerWidth < x + meWidth):
                touchSound.play()
                computerTouch = True
                eat +=1
                totalCount+=1
                computerCount +=1
                
        if gameTouch:
            drawObject(light, x + meX, y)
            mygame = pygame.image.load(random.choice(mygameImage))
            mygameSize = mygame.get_rect().size
            mygameWidth = mygameSize[0]
            mygameHeight = mygameSize[1]
            mygameX = random.randrange(0, width - mygameWidth)
            mygameY = 0
            mygameSpeed += 0.2
            gameTouch = False
            if mygameSpeed >= 10:
                mygameSpeed = 10

        if bookTouch:
            drawObject(light, x + meX, y)
            mybook = pygame.image.load(random.choice(mybookImage))
            mybookSize = mybook.get_rect().size
            mybookWidth = mybookSize[0]
            mybookHeight = mybookSize[1]
            mybookX = random.randrange(0, width - mybookWidth)
            mybookY = 0
            mybookSpeed += 0.2
            bookTouch = False
            if mygameSpeed >= 10:
                mygameSpeed = 10
            
        if computerTouch:
            drawObject(light, x + meX, y)
            mycomputer = pygame.image.load(random.choice(mycomputerImage))
            mycomputerSize = mycomputer.get_rect().size
            mycomputerWidth = mycomputerSize[0]
            mycomputerHeight = mycomputerSize[1]
            mycomputerX = random.randrange(0, width - mycomputerWidth)
            mycomputerY = 0
            mycomputerSpeed += 0.2
            computerTouch = False
            if mygameSpeed >= 10:
                mygameSpeed = 10
        
        writeScore(bookCount, gameCount, computerCount)

        mygameY += mygameSpeed
        if mygameY > height:
            mygame = pygame.image.load(random.choice(mygameImage))
            mygameSize = mygame.get_rect().size
            mygameWidth = mygameSize[0]
            mygameHeight = mygameSize[1]
            mygameX = random.randrange(0, width - mygameWidth)
            mygameY = 0
                
        mybookY += mybookSpeed
        if mybookY > height:
            mybook = pygame.image.load(random.choice(mybookImage))
            mybookSize = mybook.get_rect().size
            mybookWidth = mybookSize[0]
            mybookHeight = mybookSize[1]
            mybookX = random.randrange(0, width - mybookWidth)
            mybookY = 0

        mycomputerY += mycomputerSpeed
        if mycomputerY > height:
            mycomputer = pygame.image.load(random.choice(mycomputerImage))
            mycomputerSize = mycomputer.get_rect().size
            mycomputerWidth = mycomputerSize[0]
            mycomputerHeight = mycomputerSize[1]
            mycomputerX = random.randrange(0, width - mycomputerWidth)
            mycomputerY = 0

        if totalCount == 25:
            if grade == -50:
                ending0()
                pygame.quit
                sys.exit()
                quit()
            elif -48 <= grade < -10:
                ending1()
                pygame.quit()
                sys.exit()
                quit()
            elif -10 <= grade < 10:
                ending2()
                pygame.quit()
                sys.exit()
                quit()
            elif 10 <= grade < 20:
                ending3()
                pygame.quit()
                sys.exit()
                quit()
            elif 20 <= grade < 30:
                ending4()
                pygame.quit()
                sys.exit()
                quit()
            elif 30 <= grade < 40:
                ending5()
                pygame.quit()
                sys.exit()
                quit()
            elif 40 <= grade < 48:
                ending6()
                pygame.quit()
                sys.exit()
                quit()

        drawObject(mygame, mygameX, mygameY)

        drawObject(mybook, mybookX, mybookY)

        drawObject(mycomputer, mycomputerX, mycomputerY)

        pygame.display.update()

        clock.tick(60)

    pygame.quit()

initGame()
runGame()
