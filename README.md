- 👋 Hi, I’m @Shab44
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Shab44/Shab44 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
from biblioLDDR import*
p1=Picture(421,421)
b1=Button(text="Expérience 1")
b2=Button(width=10,text="Expérience 2")
b3=Button(width=30,text="Expérience 2 - graphique linéarisé")
b4=Button(width=20,text="Expérience 2 avec graphe")
b5 =Button(width=10,text="Expérience 3")

from pandas import*
from matplotlib.pyplot import*
import pandas as pd

def b1_click():
    global abscisse
    global ordonnee
    global titre 
    global titreordonnee 
    global titreabscissse
    global variable
    global C #intensité
    C = 6.85
    titre = "charge à courant constant"
    titreordonnee = "u[V]"
    titreabscissse = "t [s]"
    ordonnee = [] # donnee de tension 
    abscisse= [] # donnee du temps
    df = pd.read_excel("charge_a_courant_constant2.xlsx")
    for indice, ligne in df.iterrows():
        ordonnee.append(ligne["u[v]"])
        abscisse.append(ligne["t[s]"])
    yi=y1=xi=x1=0
    for a in range(0,len(abscisse)): # calcul de la pente
        if a ==1:
            y1 = abscisse[a]
        if a == len(abscisse):
            yi= abscisse[a]
    for b in range(0,len(ordonnee)):
        if b ==1:
            x1 = ordonnee[a]
        if b == len(ordonnee):
            xi= ordonnee[a]
    pente = (yi-y1)/(xi-x1)   
    fonction_droite()
    
def b2_click():
    global abscisse
    global ordonnee
    global titre 
    global titreabscisse
    global titreordonnee
    titre = "décharge lente d'un condensateur"
    titreabscisse = "u[V]"
    titreordonnee = "t [s]"
    ordonnee = [] # donnee de distance
    abscisse= [] # donnee du temps
    df = pd.read_excel("decharge_lente2.xlsx")
    for indice, ligne in df.iterrows():
        ordonnee.append(ligne["u[v]"])
        abscisse.append(ligne["t[s]"])
    
    fonction()
    
def b3_click():
    global abscisse
    global ordonnee
    global titre 
    global titreabscissse
    global titreordonnee
    global R
    titre = "décharge lente linéarisée d'un condensateur"
    titreordonnee = "ln(U/U0)"
    titreabscissse = "t [s]"
    ordonnee = [] # donnee de distance
    abscisse= [] # donnee du temps
    df = pd.read_excel("decharge_lineaire2.xlsx")
    for indice, ligne in df.iterrows():
        ordonnee.append(ligne["u[v]"])
        abscisse.append(ligne["t[s]"])
    R = 10.08 
    
    fonction_droite()
        
def b4_click(): # y =ax + b, ou b = y0
    global abscisse
    global ordonnee
    global titre 
    global titreabscissse
    global titreordonnee
    titre = "décharge lente d'un condensateur"
    titreordonnee = "u[V]"
    titreabscissse = "t [s]"
    ordonnee = [] # donnee de distance
    abscisse= [] # donnee du temps
    df = pd.read_excel("decharge_lente2.xlsx")
    for indice, ligne in df.iterrows():
        ordonnee.append(ligne["u[v]"])
        abscisse.append(ligne["t[s]"])
        
    fonction_expo()
        
                
def calcul_somme_carré(U0):
    somme_distances_carrees = 0
    for i in range(0,len(abscisse)):
        const_temps = 10.08 * 6.85
        yu0 = U0 * (1-exp(((-ordonnee[i])/const_temps)))
        somme_distances_carrees = somme_distances_carrees + ((abscisse[i] - yu0)** 2)
    print(somme_distances_carrees)
    return somme_distances_carrees
    
def b5_click(): 
    global abscisse
    global ordonnee
    global titre 
    global titreabscisse
    global titreordonnee
    global U0
    global meilleur_U0
    titre = "charge à tension constante"
    titreordonnee = "u[V]"
    titreabscisse = "t [s]" 
    ordonnee = [] # donnee de distance
    abscisse= [] # donnee du temps
    df = pd.read_excel("charge_a_tension_constante.xlsx")
    for indice, ligne in df.iterrows():
        ordonnee.append(ligne["U[V]"])
        abscisse.append(ligne["t[s]"])
        
    meilleur_U0 = 0
    c=0
    for U0 in arange(0,10,0.01):
        c = c+1
        somme_distances_carrees = calcul_somme_carré(U0)
        if c==1:
            somme_distances_carrees_min = somme_distances_carrees
        if somme_distances_carrees < somme_distances_carrees_min:
            somme_distances_carrees_min = somme_distances_carrees
            meilleur_U0 = U0
        print("Meilleur valeur de U0 :", meilleur_U0)    
    
    fonction()
        
def fonction_droite(): #droite
    figh, ax = subplots()
    plot(abscisse, ordonnee, "r+")    
    title(titre,fontsize=22)
    ax.set_ylabel(titreordonnee,
                fontsize=12,
                ha="center",
                va="center")
    ax.set_xlabel(titreabscissse,
                 fontsize=12,
                 ha="center",
                 va="center")
    
    sxy=sy=sx=my=mx=c=sx2=0
    for k in range(0,len(abscisse)):
        x1 = abscisse[k]
        y1 = ordonnee[k]
        sxy= sxy + x1*y1
        sx = sx+ x1
        sy = sy+y1
        c = c+1
        sx2 = sx2 + x1**2
    mx = sx/c
    my = sy/c
    mx2 = mx**2
    a = (sxy/c - mx*my)/(sx2/c -mx2)
    b = my -a*mx
    
    xa = abscisse[0]-1
    ya = a*xa+b
    xb = abscisse[len(abscisse)-1] +1
    yb = a*xb + b
    plot([xa,xb], [ya,yb],"b--")
    
    equation = "y=" + str(round(a,4)) + "x -" + str(round(b,4))
    xe= max(abscisse)*(3/5)
    ye= max(ordonnee)*(1/4)
    ax.text(xe,ye,equation, color = "blue", fontsize=12)
    
    if titre == "décharge lente linéarisée d'un condensateur":
        variable = "R=" + str(round(R,2)) + "MΩ"
    if titre =="charge à courant constant":
        variable = "C=" + str(round(C,2)) + "μF"
    equation =  variable 
    xe= max(abscisse)*(3/5)
    ye= max(ordonnee)*(1/8)
    ax.text(xe,ye,equation, color = "red", fontsize=12)
    
    c = 0 # plus grande valeur sur l'ordonnee
    for o in ordonnee:
        c = c+1
        if c == 1:
            omax = o
        if o > omax:
            omax = o
    axis([0,abscisse[len(abscisse)-1] + 0.05*abscisse[len(abscisse)-1],0,omax+ 0.05*omax])
    grid(linestyle="--")
    gca().xaxis.set_minor_locator(MultipleLocator(4))
    gca().yaxis.set_minor_locator(MultipleLocator(0.5))
    
    tick_params(axis='x', which='both',
                top=False,)
    tick_params(axis='y', which='both', 
                right=False,)
    for pos in ['right', 'top']:
        gca().spines[pos].set_visible(False)
        
    show()
    
def fonction_expo(): #exponentielle
    figh, ax = subplots()
    plot(abscisse, ordonnee, "r+")    
    title(titre,fontsize=22)
    ax.set_ylabel(titreordonnee,
                fontsize=12,
                ha="center",
                va="center")
    ax.set_xlabel(titreabscissse,
                 fontsize=12,
                 ha="center",
                 va="center")
    b = 0.013965
    c = 0 # plus grande valeur sur l'abscisse
    for k in range (0, len(abscisse)):
        if abscisse[k]==0:
            N0 = ordonnee[k]
    for a in abscisse:
        c = c+1
        if c == 1:
            amax = a
        if a > amax:
            amax = a
    xa = linspace(0,amax,200)
    ya = N0*(exp(xa*(-b))) #formule d'une exponentielle
    plot(xa,ya,"b--")
    
    c = 0 # plus grande valeur sur l'ordonnee
    for o in ordonnee:
        c = c+1
        if c == 1:
            omax = o
        if o > omax:
            omax = o
    axis([0,abscisse[len(abscisse)-1] + 0.05*abscisse[len(abscisse)-1],0,omax+ 0.05*omax])
    grid(linestyle="--")
    gca().xaxis.set_minor_locator(MultipleLocator(4))
    gca().yaxis.set_minor_locator(MultipleLocator(0.5))
    
    tick_params(axis='x', which='both',
                top=False,)
    tick_params(axis='y', which='both', 
                right=False,)
    for pos in ['right', 'top']:
        gca().spines[pos].set_visible(False)
    show()

def fonction(): # courbe, sans courbe de tendance
    figh, ax = subplots()
    plot(abscisse, ordonnee, "r+")    
    title(titre,fontsize=22)
    ax.set_ylabel(titreordonnee,
                fontsize=12,
                ha="center",
                va="center")
    ax.set_xlabel(titreabscisse,
                 fontsize=12,
                 ha="center",
                 va="center")
    grid(linestyle="--")
    gca().xaxis.set_minor_locator(MultipleLocator(4))
    gca().yaxis.set_minor_locator(MultipleLocator(0.5))
    
    if titre == "charge à tension constante":
        const_temps = 10.08 * 6.85 # Resistance * capacité
        equation = "τ =" + str(round(const_temps,4))
        xe= max(abscisse)*(3/5)
        ye= max(ordonnee)*(1/4)
        ax.text(xe,ye,equation, color = "red", fontsize=12)
    
        xa = linspace(0,350,700)
        ya = meilleur_U0 * (1-exp((-xa/const_temps)))
        plot(xa,ya,"b--")
        
        equation = "U0=" + str(round(meilleur_U0,4)) + "V"
        xe= max(abscisse)*(3/5)
        ye= max(ordonnee)*(1/8)
        ax.text(xe,ye,equation, color = "red", fontsize=12)
    
    tick_params(axis='x', which='both',
                top=False,)
    tick_params(axis='y', which='both', 
                right=False,)
    for pos in ['right', 'top']:
        gca().spines[pos].set_visible(False)
    
    c = 0 # plus grande valeur sur l'ordonnee
    for o in ordonnee:
        c = c+1
        if c == 1:
            omax = o
        if o > omax:
            omax = o
    axis([0,abscisse[len(abscisse)-1] + 0.05*abscisse[len(abscisse)-1],0,omax+ 0.05*omax])

        
    show()
    
        
widgetsArray([[b2],[b1,b3,b5],[b4]])

