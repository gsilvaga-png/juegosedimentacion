import pygame, sys

pygame.init()
W,H=1100,700
screen=pygame.display.set_mode((W,H))
pygame.display.set_caption("¿Quién quiere ser Millonario?")
clock=pygame.time.Clock()

WHITE=(255,255,255);YELLOW=(255,215,0);BLUE=(20,35,100);DARK=(10,15,50)
GREEN=(40,180,70);RED=(200,50,50);GOLD=(180,140,0)

font=pygame.font.SysFont("arial",28)
big=pygame.font.SysFont("arial",42,True)
title=pygame.font.SysFont("arial",54,True)
qfont=pygame.font.SysFont("arial",32,True)
optfont=pygame.font.SysFont("arial",24)

preguntas=[
{"pregunta":"¿Qué es la sedimentación?","opciones":["Separación de partículas sólidas de un líquido por gravedad","Evaporación del agua","Mezcla de dos líquidos","Calentamiento de una sustancia"],"respuesta":0},
{"pregunta":"¿Qué fuerza es la principal responsable de la sedimentación?","opciones":["Fuerza centrífuga","Gravedad","Presión atmosférica","Tensión superficial"],"respuesta":1},
{"pregunta":"¿Cuál de estas partículas sedimenta más rápido, en igualdad de condiciones?","opciones":["Una partícula pequeña y ligera","Una partícula hueca","Una burbuja de aire","Una partícula grande y densa"],"respuesta":3},
{"pregunta":"¿En qué tipo de tratamiento se usa comúnmente la sedimentación?","opciones":["Tratamiento de agua potable y residual","Climatización","Refrigeración industrial","Generación eléctrica"],"respuesta":0},
{"pregunta":"En un sedimentador, ¿cómo se llama el material sólido que se deposita en el fondo?","opciones":["Sobrenadante","Espuma","Lodo (o lodos)","Efluente"],"respuesta":2},
{"pregunta":"Según la Ley de Stokes, la velocidad de sedimentación de una partícula esférica depende principalmente de:","opciones":["Solo su color","Su diámetro, la diferencia de densidades y la viscosidad del fluido","Solo la temperatura del ambiente","El pH del agua"],"respuesta":1},
{"pregunta":"Si aumenta la viscosidad del fluido, la velocidad de sedimentación de una partícula:","opciones":["Aumenta","No cambia","Disminuye","Se vuelve negativa"],"respuesta":2},
{"pregunta":"¿Cuál de las siguientes NO es un tipo de sedimentación reconocido en la teoría?","opciones":["Sedimentación discreta","Sedimentación floculenta","Sedimentación zonal","Sedimentación catalítica"],"respuesta":3},
{"pregunta":"En la sedimentación floculenta, las partículas:","opciones":["Se aglomeran entre sí formando flóculos más grandes que sedimentan más rápido","Sedimentan de forma independiente sin interactuar","Permanecen siempre en suspensión","Se disuelven completamente"],"respuesta":0},
{"pregunta":"¿Qué parámetro determina la eficiencia de un sedimentador según la tasa de desbordamiento superficial?","opciones":["El color del tanque","La relación entre el caudal y el área superficial del sedimentador","El material del tanque","La hora del día"],"respuesta":1},
]

premios=[100,200,300,500,1000,2000,4000,8000,16000,1000000]
estado="MENU";idx=0
puntaje=0

def txt(s,f,c,x,y,center=False):
    i=f.render(s,1,c)
    r=i.get_rect(center=(x,y)) if center else i.get_rect(topleft=(x,y))
    screen.blit(i,r)

def wrap_text(text,f,max_width):
    palabras=text.split(" ")
    lineas=[];actual=""
    for palabra in palabras:
        prueba=(actual+" "+palabra).strip()
        if f.size(prueba)[0]<=max_width:
            actual=prueba
        else:
            if actual:
                lineas.append(actual)
            actual=palabra
    if actual:
        lineas.append(actual)
    return lineas

def draw_multiline(lineas,f,c,x,y,line_height,center=False):
    for n,linea in enumerate(lineas):
        txt(linea,f,c,x,y+n*line_height,center)

def mensaje(s,c):
    o=pygame.Surface((W,H));o.set_alpha(170);o.fill((0,0,0));screen.blit(o,(0,0))
    txt(s,title,c,W//2,H//2,True)
    pygame.display.flip();pygame.time.delay(900)

while True:
    mx,my=pygame.mouse.get_pos()
    screen.fill(DARK if estado!="JUEGO" else BLUE)

    if estado=="MENU":
        txt("¿QUIÉN QUIERE SER",title,YELLOW,W//2,170,True)
        txt("MILLONARIO?",title,YELLOW,W//2,240,True)
        play=pygame.Rect(W//2-150,420,300,70)
        col=GREEN if play.collidepoint((mx,my)) else GOLD
        pygame.draw.rect(screen,col,play,border_radius=20)
        txt("COMENZAR",big,WHITE,W//2,455,True)

    elif estado=="JUEGO":
        q=preguntas[idx]
        txt(f"Pregunta {idx+1}/10",font,YELLOW,20,20)
        txt(f"${premios[idx]}",font,WHITE,930,20)

        qlineas=wrap_text(q["pregunta"],qfont,1000)
        draw_multiline(qlineas,qfont,WHITE,50,75,38)
        opt_top=75+len(qlineas)*38+25

        rects=[]
        row_h=90
        for i,opt in enumerate(q["opciones"]):
            r=pygame.Rect(70,opt_top+i*(row_h+14),930,row_h)
            hover=r.collidepoint((mx,my))
            pygame.draw.rect(screen,YELLOW if hover else GOLD,r,border_radius=18)
            pygame.draw.rect(screen,WHITE,r,2,border_radius=18)
            olineas=wrap_text(f"{'ABCD'[i]}) {opt}",optfont,r.width-40)
            total_h=len(olineas)*28
            oy=r.y+(row_h-total_h)//2
            draw_multiline(olineas,optfont,DARK,r.x+20,oy,28)
            rects.append(r)

    elif estado=="WIN":
        txt("¡JUEGO TERMINADO!",title,GREEN,W//2,200,True)
        txt(f"Respuestas correctas: {puntaje}/{len(preguntas)}",big,WHITE,W//2,300,True)
        porcentaje=puntaje*100//len(preguntas)
        txt(f"Porcentaje: {porcentaje}%",big,WHITE,W//2,360,True)

    pygame.display.flip()

    for e in pygame.event.get():
        if e.type==pygame.QUIT:
            pygame.quit();sys.exit()
        if e.type==pygame.MOUSEBUTTONDOWN:
            if estado=="MENU" and play.collidepoint(e.pos):
                estado="JUEGO"
            elif estado=="JUEGO":
                for i,r in enumerate(rects):
                    if r.collidepoint(e.pos):
                        ok=i==preguntas[idx]["respuesta"]
                        for k in range(5):
                            pygame.draw.rect(screen,GREEN if ok else RED,r.inflate(k*8,k*8),border_radius=20)
                            pygame.display.flip();pygame.time.delay(40)
                            screen.fill(BLUE)

                        if ok:
                            mensaje("¡CORRECTO!", GREEN)
                            puntaje += 1
                        else:
                            mensaje("¡INCORRECTO!", RED)

                        idx += 1

                        if idx >= len(preguntas):
                            estado = "WIN"
                        else:
                            estado = "JUEGO"
            elif estado=="WIN":
                pygame.quit();sys.exit()
    clock.tick(60)
