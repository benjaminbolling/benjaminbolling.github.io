---
layout: default
---

# Hello !

<h1 align="center">
    <img src="https://readme-typing-svg.herokuapp.com/?font=Righteous&size=35&center=true&vCenter=true&width=1000&height=200&duration=4000&pause=500&color=2C98C8FF&lines=Dear+Visitor,;Welcome+to+my+personal+coding+page!+👋;If+you+have+any+requests+or+questions,+drop+me+an+email.;" />
</h1>

## Some info
I will keep this minimalistic. My GitHub profile contains more detailed information (click stats link below).

[![GitHub stats](https://github-readme-stats.vercel.app/api?username=benjaminbolling&theme=solarized-dark&show_icons=true&rank_icon=github)](https://github.com/benjaminbolling)


[LU Research Portal](https://portal.research.lu.se/en/persons/benjamin-bolling)

[![LinkedIn](https://a11ybadges.com/badge?logo=linkedin)](https://www.linkedin.com/in/benjamin-b-a320475b/)

[![ORCID](https://a11ybadges.com/badge?logo=orcid)](https://orcid.org/0000-0002-6650-5365)

## Current info
- Control Room Shift Leader at European Spallation Source
- PhD student in Physics at European Spallation Source/Lund University

## Fun simple application
Because... Why not. Written in Python, went overkill for a course at KTH. 

Good for learning both the periodic table and Swedish (or either). Input files:
- [atomegenskaper.txt](assets/pyex/atomegenskaper.txt)
- [atomer.txt](assets/pyex/atomer.txt)
- [poanglista.txt](assets/pyex/poanglista.txt)

```python
from tkinter import *
from tkinter import messagebox, ttk
from datetime import datetime
from random import randint, shuffle
from tkinter.simpledialog import askstring
from copy import deepcopy

# En klass som beskriver en atom:
#    tecken - vilket tecken atomen har av datatypen sträng
#    namn - vilket namm atomen har (på svenska) av datatypen sträng
#    atomnummer - vilket atomnummer atomen har av datatypen heltal
#    atommassa - atomens atommassa (enhet u) av datatypen flytvärde
#    kolumn - atomens kolumn i periodiska systemet av datatypen heltal
#    rad - atomens rad i periodiska systemet av datatypen heltal
#    atomegenskaper - egenskaper kring olika atomer (kolumn, ämnesklass och aggregationstillstånd) av datatypen dictionary

class Atom:
    def __init__(self, tecken, namn, atomnummer, atommassa, atomegenskaper):
        self.tecken = tecken
        self.namn = namn
        self.atomnummer = atomnummer
        self.atommassa = atommassa
        self.atomegenskaper = atomegenskaper
        self.kolumn = atomegenskaper['Kolumn i Periodiska Systemet']
        self.rad = atomegenskaper['Rad i Periodiska Systemet']
    
    def __lt__(self,other):
        if self.namn < other.namn:
            return True
        else:
            return False

    def __str__(self):
        atominfo = []
        atominfo.append(('(namn (tecken))({} ({}))'.format(self.namn,self.tecken)))
        atominfo.append(('(atomnummer)({})'.format(self.atomnummer)))
        atominfo.append(('(atommassa)({})'.format(self.atommassa)))
        for metadata, data in self.atomegenskaper.items():
            atominfo.append(('({})({})'.format(metadata,data)))
        return '\n'.join(atominfo)

# En klass som låter spelaren bygga upp periodiska systemet från grunden
#    poänglista - en lista med högsta poäng för speltypen ByggPeriodiskaSystemet av datatypen lista
#    atomer - en lista som innehåller samtliga atomer

class ByggPeriodiskaSystemet(Tk):
    def __init__(self, poänglista, atomer):
        super().__init__()
        self.title("Periodiska Systemet 178 - Bygg Periodiska Systemet")
        self.poänglista = poänglista
        self.antal_felplaceringar = 0
        self.atomer = deepcopy(atomer)
        self.bygg_huvudanvändargränsnitt()
        self.atomlista = []
        for a in atomer:
            self.atomlista.append([a.tecken, a.namn])
        shuffle(self.atomlista)
        self.atom_id = -1
        self.nästa_atom()

    def nästa_atom(self):
        self.atom_id += 1
        self.nästaAtom.config(state=DISABLED)
        if self.atom_id < len(self.atomlista):
            self.frågeText.config(text='Vilken placering skall {} ({}) ha?'.format(self.atomlista[self.atom_id][0],self.atomlista[self.atom_id][1]))
            for atomKnapp in self.atomKnappar:
                atomKnapp.config(highlightbackground=self.cget('bg'), state=NORMAL)
            self.atom_att_placera = self.atomlista[self.atom_id][0]
        else:
            self.vinst_och_avsluta()

    def avsluta_bygg_periodiska_systemet(self):
        '''
        Avslutar spelomgången, frågemeddelande ges där användaren får bekräfta avsluta.
        Inparametrar: self
        Returnerar: ingenting
        '''
        if messagebox.askyesno(title='Varning', message='Vill du verkligen avsluta spelomgången?', icon='warning'):
            self.quit()

    def acceptera_atoms_placering(self, a_index):
        if self.atom_att_placera == self.atomer[a_index].tecken:
            self.nästaAtom.config(state=NORMAL)
            self.atomKnappar[a_index].config(text=self.atom_att_placera)
            for atomKnapp in self.atomKnappar:
                atomKnapp.config(state=DISABLED)
            self.atomKnappar[a_index].config(highlightbackground='green')
        else:
            self.antal_felplaceringar += 1
            self.atomKnappar[a_index].config(highlightbackground='red', state=DISABLED)
        self.uppdatera_huvudanvändargränsnitt()

    def uppdatera_huvudanvändargränsnitt(self):
        self.antaletfelText.config(text = 'Antalet felplaceringar: {}'.format(self.antal_felplaceringar))

    def bygg_huvudanvändargränsnitt(self):
        self.mainframe = ttk.Frame(self, padding="3 3 12 12")
        self.mainframe.grid(column=0, row=0, sticky=(N, W, E, S))

        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)
        
        self.nästaAtom = Button(self.mainframe, text="Nästa Atom", command = self.nästa_atom)
        self.nästaAtom.grid(column=14, columnspan = 2, row=1, sticky=(W, E))

        Button(self.mainframe, text="Återgå till Huvudmeny", command = self.avsluta_bygg_periodiska_systemet).grid(column=16, columnspan = 3, row=1, sticky=(W, E))
        self.frågeText = Label(self.mainframe, text = 'Frågetext')
        self.frågeText.grid(column=1, columnspan = 8, row=1, sticky='W')

        self.antaletfelText = Label(self.mainframe, text = 'Antalet felplaceringar: 0')
        self.antaletfelText.grid(column=10, columnspan = 3, row=1, sticky='W')

        self.atomKnappar = []

        for rad in range(1,10):
            if rad < 8:
                mrad = rad + 2
            else:
                if rad == 8:
                    Label(self.mainframe, text = ' - ').grid(column=0, row=rad+2, sticky=(W, E))
                mrad = rad + 3
            Label(self.mainframe, text = rad).grid(column=0, row=mrad, sticky=(W, E))
            
        for kolumn in range(1,19):
            Label(self.mainframe, text = kolumn).grid(column=kolumn, row=2, sticky=(W, E))
        for a_index, a in enumerate(self.atomer):
            for kolumn in range(1,19):
                if a.kolumn == str(kolumn):
                    for rad in range(1,10):
                        if a.rad == str(rad):
                            if rad < 8:
                                mrad = rad + 2
                            else:
                                if rad == 8:
                                    Label(self.mainframe, text = ' - ').grid(column=kolumn, row=rad+2, sticky=(W, E))
                                mrad = rad + 3
                            self.atomKnappar.append(Button(self.mainframe, text='  ', width=2, command = lambda a_index=a_index: self.acceptera_atoms_placering(a_index)))
                            self.atomKnappar[a_index].grid(column=kolumn, row=mrad, sticky=(W, E))
        
    def vinst_och_avsluta(self):
        if self.antal_felplaceringar < int(self.poänglista['ByggPeriodiskaSystemet'][-1][0]):
            tidsstämpel = datetime.now()
            spelarnamn = askstring('Namn', 'Grattis, du klarade att placera ut alla atomerna och kvalificerade dig till topplistan! Ange ditt namn:')
            self.poänglista['ByggPeriodiskaSystemet'].append([str(self.antal_felplaceringar),tidsstämpel,spelarnamn])
            self.poänglista['ByggPeriodiskaSystemet'].sort()
            self.poänglista['ByggPeriodiskaSystemet'][:-1]
        else:
            messagebox.showinfo(title='Vinst!', message='Bra jobbat, du klarade att placera ut alla atomerna! Dock gjorde du för många fel för att kvalificera dit till topplistan.')
        self.quit()

# En klass som beskriver en frågesportsspels frågedialog
#    fråga - sträng
#    svarsalternativ - lista
#    liv kvar - heltal
#    poäng (hur många frågor som korrekt besvarats) - heltal
#    chanser kvar på frågan - heltal

class FrågeDialog(Toplevel):
    def __init__(self,parent,fråga,svarsalternativ,poäng,liv_kvar,chanser,antal_frågor):
        Toplevel.__init__(self,parent)
        self.title('Frågesport')
        self.fråga = fråga
        self.transient(parent)
        self.protocol("WM_DELETE_WINDOW",self.avsluta_frågedialog)
        self.svarsalternativ = svarsalternativ
        self.poäng = poäng
        self.liv_kvar = liv_kvar
        self.chanser = chanser
        self.frågor_kvar = antal_frågor - poäng

        self.bygg_fråga_användargränsnitt()
        self.grab_set()
        self.wait_window()
        
    def bygg_fråga_användargränsnitt(self):
        frame = Frame(self)
        frame.grid(column=0, row=0, sticky=(N, W, E, S))
        Label(frame,text='Poäng: {}'.format(self.poäng)).grid(column=1, row = 1, sticky=(W, E))
        Label(frame,text='Liv kvar: {}'.format(self.liv_kvar)).grid(column=2, row = 1, sticky=(W, E))
        Label(frame,text='Chanser kvar: {}'.format(self.chanser)).grid(column=3, columnspan=2, row = 1, sticky=(W, E))
        Label(frame,text='Frågor kvar (inkl. denna): {}'.format(self.frågor_kvar)).grid(column=3, columnspan=4, row = 2, sticky=(W, E))

        Label(frame,text=self.fråga).grid(column=1, columnspan=4, row = 3, sticky=(W, E))
        frame.grid(row=1)
        kolumn = 0
        for valt_alternativ in self.svarsalternativ:
            kolumn += 1
            Button(frame, text=valt_alternativ, width=20, command = lambda valt_alternativ=valt_alternativ: self.alternativ_valt(valt_alternativ)).grid(column=kolumn, row=4, sticky=(W, E))
        
    def alternativ_valt(self,valt_alternativ):
        self.valt_alternativ = valt_alternativ
        self.destroy()
        
    def avsluta_frågedialog(self):
        if messagebox.askyesno(title='Varning', message='Vill du verkligen avsluta spelomgången?', icon='warning'):
            self.valt_alternativ = None
            self.destroy()

# En klass som beskriver en frågesportsspelomgång
#    poänglista - en lista med högsta poäng för speltypen Frågesport av datatypen lista
#    atomer - en lista som innehåller samtliga atomer

class Frågesport(Tk):
    def __init__(self, poänglista, atomer):
        super().__init__()
        self.title("Periodiska Systemet 178 - Frågesport")
        self.poänglista = poänglista
        self.atomer = deepcopy(atomer)
        self.bygg_frågelistan()
        self.bygg_uppstarts_användargränsnitt()
        self.visa_uppstarts_användargränsnitt()

    def bygg_frågelistan(self):
        frågor = []
        frågetyper_svarsalternativ = {}
        frågetyper_svarsalternativ['tecken'] = []
        frågetyper_svarsalternativ['namn'] = []
        frågetyper_svarsalternativ['atomnummer'] = []
        frågetyper_svarsalternativ['atommassa'] = []
        frågetyper_svarsalternativ['kolumn'] = []
        frågetyper_svarsalternativ['rad'] = []

        for a in self.atomer:
            frågetyper_svarsalternativ['tecken'].append(a.tecken)
            frågetyper_svarsalternativ['namn'].append(a.namn)
            frågetyper_svarsalternativ['atomnummer'].append(a.atomnummer)
            frågetyper_svarsalternativ['atommassa'].append(a.atommassa)
            frågetyper_svarsalternativ['kolumn'].append(a.kolumn)
            frågetyper_svarsalternativ['rad'].append(a.kolumn)
            for egenskap, värde in a.atomegenskaper.items():
                if egenskap not in frågetyper_svarsalternativ.keys():
                    frågetyper_svarsalternativ[egenskap] = []
                if värde not in frågetyper_svarsalternativ[egenskap]:
                    frågetyper_svarsalternativ[egenskap].append(värde)
        
        for a in self.atomer:
            a.atomegenskaper['tecken'] = a.tecken
            a.atomegenskaper['atomnummer'] = a.atomnummer
            a.atomegenskaper['atommassa'] = a.atommassa
            a.atomegenskaper['kolumn'] = a.kolumn
            a.atomegenskaper['rad'] = a.rad
            
            # Gissa atomens namn givet atomens tecken, atomnummer eller atommassa
            for egenskap in ['tecken','atomnummer','atommassa']:
                shuffle(frågetyper_svarsalternativ['namn'])
                alternativ = frågetyper_svarsalternativ['namn'][:3]
                värde = a.atomegenskaper[egenskap]
                korrekt_svar = a.namn
                if korrekt_svar in alternativ:
                    korrekt_id = alternativ.index(korrekt_svar)
                else:
                    korrekt_id = randint(0,2)
                    alternativ[korrekt_id] = korrekt_svar
                frågor.append(['Vilken atom har {} {}?'.format(egenskap,värde),[alternativ],korrekt_id])

            # Gissa atomegenskapsvärdet givet atomens namn.
            for egenskap in a.atomegenskaper.keys():
                shuffle(frågetyper_svarsalternativ[egenskap])
                alternativ = frågetyper_svarsalternativ[egenskap][:3]
                korrekt_svar = a.atomegenskaper[egenskap]
                if korrekt_svar in alternativ:
                    korrekt_id = alternativ.index(korrekt_svar)
                else:
                    korrekt_id = randint(0,2)
                    alternativ[korrekt_id] = korrekt_svar
                frågor.append(['Vilket {} har {}?'.format(egenskap,a.namn),[alternativ],korrekt_id])
                
        self.frågor = frågor

    def bygg_spelomgång(self):
        self.uppstartsframe.grid_forget()
        shuffle(self.frågor)
        liv_kvar = 10 # Som en katt
        poäng = 0
        respons = -1
        for fråga in self.frågor:
            chanser = 2
            while respons != fråga[1][0][fråga[2]] and respons != None and chanser > 0 and liv_kvar > 0:
                respons = FrågeDialog(self,fråga[0],fråga[1][0],poäng,liv_kvar,chanser,len(self.frågor)).valt_alternativ
                if respons != fråga[1][0][fråga[2]]:
                    chanser -= 1
                    liv_kvar -= 1
                    if respons != None and chanser > 0 and liv_kvar > 0:
                        self.visa_fel_val_meddelande(respons, chanser, liv_kvar)
                else:
                    poäng += 1
            if chanser == 0:
                self.visa_chanser_slut(fråga[0], fråga[1][0][fråga[2]])
                liv_kvar = 0
        if respons != None and liv_kvar != 0:
            self.vinst_frågesport()
        elif respons != None:
            self.spelet_slut()
        self.uppdatera_poänglistan(poäng)
        self.visa_uppstarts_användargränsnitt()

    def uppdatera_poänglistan(self, poäng):
        if poäng > int(self.poänglista['Frågesport'][-1][0]):
            tidsstämpel = datetime.now()
            spelarnamn = askstring('Namn', 'Grattis, du kvalificerade dig till topplistan! Ange ditt namn:')
            self.poänglista['Frågesport'].append([str(poäng),tidsstämpel,spelarnamn])
            self.poänglista['Frågesport'].sort(reverse = True)
            self.poänglista['Frågesport'][:-1]

    def spelet_slut(self):
        messagebox.showinfo(title='Game Over', message='Bra jobbat, men där tog spelet slut för dig!')

    def vinst_frågesport(self):
        messagebox.showinfo(title='Game Defeat', message='Fantastiskt bra jobbat, du har besegrat spelet!')

    def huvudmeny(self):
        self.quit()

    def visa_uppstarts_användargränsnitt(self):
        for i, poänglista_kolumn in enumerate(self.poänglista_grid[1:]):
            for j, rad in enumerate(poänglista_kolumn):
                if i == 0:
                    rad.config(text = '{}'.format(self.poänglista['Frågesport'][j][0]))
                elif i == 1:
                    rad.config(text = '{}'.format(self.poänglista['Frågesport'][j][1].strftime("%Y-%m-%d %H:%M:%S")))
                else:
                    rad.config(text = '{}'.format(self.poänglista['Frågesport'][j][2]))
        self.uppstartsframe.grid()

    def bygg_uppstarts_användargränsnitt(self):
        self.uppstartsframe = ttk.Frame(self, padding="3 3 12 12")
        self.uppstartsframe.grid(column=0, row=0, sticky=(N, W, E, S))

        Label(self.uppstartsframe, text = 'Dags för frågesport!').grid(column=1, columnspan=6, row=1)
        Button(self.uppstartsframe, text="Starta spelet!", width=20, command = self.bygg_spelomgång).grid(column=3, row=2, columnspan=2, sticky=(W, E))
        
        self.poänglista_grid = []

        poänglista_kolumn = []
        poänglista_kolumn.append(Label(self.uppstartsframe, text = 'Poäng'))
        poänglista_kolumn[-1].grid(column=1, columnspan=2, row=3)
        poänglista_kolumn.append(Label(self.uppstartsframe, text = 'Datum'))
        poänglista_kolumn[-1].grid(column=3, columnspan=2, row=3)
        poänglista_kolumn.append(Label(self.uppstartsframe, text = 'Namn'))
        poänglista_kolumn[-1].grid(column=5, columnspan=2, row=3)

        self.poänglista_grid.append(poänglista_kolumn)
        for kolumn in [1,3,5]:
            poänglista_kolumn = []
            for rad in range(4,14):
                poänglista_kolumn.append(Label(self.uppstartsframe, text = ''))
                poänglista_kolumn[-1].grid(column=kolumn, columnspan=2, row=rad)
            self.poänglista_grid.append(poänglista_kolumn)
        
        Button(self.uppstartsframe, text="Hjälp", width=20, command = self.hjälp).grid(column=3, row=14, columnspan=2, sticky=(W, E))
        Button(self.uppstartsframe, text="Huvudmeny", width=20, command = self.huvudmeny).grid(column=3, row=15, columnspan=2, sticky=(W, E))

    def hjälp(self):
        meddelande = []
        meddelande.append('Klicka på Starta spelet, så kommer det att komma en slumpvis fråga varje gång.')
        meddelande.append('Svarar du korrekt, får du 1 poäng.')
        meddelande.append('Du har totalt 10 liv; varje gång du svarar fel förlorar du ett liv.')
        meddelande.append('Dessutom måste du klara varje fråga. Du har 3 chanser per fråga.')
        meddelande.append('Har du förbrukat dina 10 liv eller 3 chanser på en fråga är spelet över.')
        meddelande.append('Då sammanställs dina poäng och om du har nått tillräckligt med poäng hamnar du på topplistan')
        messagebox.showinfo(title='Hjälp - Frågesport!', message='\n'.join(meddelande))

    def visa_fel_val_meddelande(self, valt_alternativ, chanser_kvar, liv_kvar):
        messagebox.showerror(title='Fel svar!', message='Du svarade {}, vilket är fel svar. Du har {} chanser och {} liv kvar. Försök igen!'.format(valt_alternativ, chanser_kvar, liv_kvar), icon='warning')

    def visa_chanser_slut(self, fråga, korrekt_svarsalternativ):
        messagebox.showerror(title='Fel svar!', message='Fel svar på frågan: "{}". Rätt svar är {}.'.format(fråga, korrekt_svarsalternativ), icon='warning')

# En klass som beskriver en läsomgång där spelaren kan läsa om olika atomer.
#    atomer - en lista som innehåller samtliga Atominstanser

class Läsning(Tk):
    def __init__(self, atomer, max_atominfo):
        super().__init__()
        self.max_atominfo = max_atominfo # Kan utökas vid behov
        self.title("Periodiska Systemet 178 - Atomläsning")
        self.atomer = deepcopy(atomer)
        self.protocol("WM_DELETE_WINDOW", self.återgå)
        self.menyTyp = 'huvudmeny'
        self.bygg_atominfo_användargränsnitt()
        self.bygg_listmeny_användargränsnitt()
        self.bygg_huvudmeny_användargränsnitt()

    def bygg_atominfo_användargränsnitt(self):
        self.atominfoframe = ttk.Frame(self, padding="3 3 12 12")
        self.atominfoframe.grid(column=0, row=0, sticky=(N, W, E, S))
        
        self.atominfo_objektlista = []
        kol_id = -1
        for kolumn in range(1,6):
            if kolumn != 3:
                kol_id += 1
                self.atominfo_objektlista.append([])
            for rad in range(0,int(int(self.max_atominfo/2) + (self.max_atominfo % 2 > 0))):
                if kolumn == 3:
                    Label(self.atominfoframe, text = ' :: ').grid(column=3, row=rad+2)
                else:
                    textorientering = "E" if kolumn == 1 or kolumn == 4 else "W"
                    atominfo_objekt = Label(self.atominfoframe, text = '')
                    atominfo_objekt.grid(column=kolumn, row=rad+2, sticky=textorientering)
                    self.atominfo_objektlista[kol_id].append(atominfo_objekt)
        
        Label(self.atominfoframe, text = ' ').grid(column=0, row=1)
        ttk.Separator(self.atominfoframe, orient='horizontal').grid(row=1, columnspan = 19, sticky=(W, E))
        self.gömatominfo = Button(self.atominfoframe, text="Göm atomen", width=8, command = self.göm_atomen)
        self.gömatominfo.grid(column=6, row=1, sticky=(W, E))

        self.atominfoframe.grid_forget()

    def byt_menytyp(self):
        if self.menyTyp == 'lista':
            self.menyTyp = 'huvudmeny'
            self.listframe.grid_forget()
            self.mainframe.grid()
        else:
            self.menyTyp = 'lista'
            self.listframe.grid()
            self.mainframe.grid_forget()

    def bygg_listmeny_användargränsnitt(self):
        self.listframe = ttk.Frame(self, padding="3 3 12 12")
        self.listframe.grid(column=0, row=0, sticky=(N, W, E, S))

        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)
        
        Button(self.listframe, text="Huvudmeny", command = self.byt_menytyp).grid(column=1, columnspan = 3, row=1, sticky=(W, E))
        Label(self.listframe, text = 'Öva dina kunskaper vad gäller periodiska systemet! Klicka på en atom för mer information.').grid(column=4, columnspan = 11, row=1, sticky=(W, E))
        Button(self.listframe, text="Återgå", command = self.återgå).grid(column=14, columnspan = 2, row=1, sticky=(W, E))

        atomer = self.atomer
        atomer.sort()
        i = 0

        Label(self.listframe, text = ' ').grid(column=0, row=2)
        ttk.Separator(self.listframe, orient='horizontal').grid(row=2, columnspan = 19, sticky=(W, E))

        for kolumn in range(15):
            for rad in range(8):
                if i < len(atomer):
                    a = atomer[i]
                    Button(self.listframe, text=a.namn, command = lambda tecken=a.tecken: self.visa_atomen(tecken)).grid(column=kolumn+1, row=rad+3, sticky='W')
                    i += 1
        
        self.listframe.grid_forget()

    def bygg_huvudmeny_användargränsnitt(self):
        self.mainframe = ttk.Frame(self, padding="3 3 12 12")
        self.mainframe.grid(column=0, row=0, sticky=(N, W, E, S))

        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)
        
        Label(self.mainframe, text = '⚛', font=("Arial", 25)).grid(column=0, row=1, sticky=(W, E))
        Button(self.mainframe, text="Listmeny", command = self.byt_menytyp).grid(column=1, columnspan = 3, row=1, sticky=(W, E))
        Label(self.mainframe, text = 'Öva dina kunskaper vad gäller periodiska systemet! Klicka på en atom för mer information.').grid(column=4, columnspan = 12, row=1, sticky=(W, E))
        Button(self.mainframe, text="Återgå", command = self.återgå).grid(column=16, columnspan = 3, row=1, sticky=(W, E))

        Label(self.mainframe, text = 'Grupp →').grid(column=0, row=3, sticky=(W, E))
        Label(self.mainframe, text = 'Period ↓').grid(column=0, row=4, sticky=(W, E))

        Label(self.mainframe, text = ' ').grid(column=0, row=2)
        ttk.Separator(self.mainframe, orient='horizontal').grid(row=2, columnspan = 19, sticky=(W, E))

        for kolumn in range(1,19):
            Label(self.mainframe, text = str(kolumn)).grid(column=kolumn, row=3, sticky=(W, E))
        for a in self.atomer:
            for kolumn in range(1,19):
                if a.kolumn == str(kolumn):
                    for rad in range(1,10):
                        if a.rad == str(rad):
                            if rad < 8:
                                mrad = rad + 4
                                Label(self.mainframe, text = str(rad)).grid(column=0, row=mrad, sticky=(W, E))
                            else:
                                mrad = rad + 5
                            Button(self.mainframe, text=a.tecken, width=2, command = lambda tecken=a.tecken: self.visa_atomen(tecken)).grid(column=kolumn, row=mrad, sticky=(W, E))
        
        Label(self.mainframe, text = ' ').grid(column=0, row=12)
        ttk.Separator(self.mainframe, orient='horizontal').grid(row=12, column = 3, columnspan = 14, sticky=(W, E))
        
        Label(self.mainframe, text = '*').grid(column=0, row=13, sticky=(W, E))
        Label(self.mainframe, text = '**').grid(column=0, row=14, sticky=(W, E))

        Label(self.mainframe, text = ' ').grid(column=0, row=15)
        ttk.Separator(self.mainframe, orient='horizontal').grid(row=15, columnspan = 19, sticky=(W, E))

        Label(self.mainframe, text = 'Perioderna * och ** tillhör perioderna 6 resp. 7.').grid(column=0, columnspan = 18, row=16, sticky=(W))

    def ladda_atomen(self,vald_atom):
        return [(a.split(')(')[0][1:],a.split(')(')[1][:-1]) for a in str(vald_atom).split('\n')]

    def visa_atomen(self,atomtecken):
        self.mainframe.grid_forget()
        self.atominfoframe.grid()
        for a in self.atomer:
            if a.tecken == atomtecken:
                atomdata = self.ladda_atomen(a)
                radantal = int(int(len(atomdata)/2) + (len(atomdata) % 2 > 0))
                i = 0
                for kolumn in [0,2]:
                    for rad in range(0,radantal):
                        if i < len(atomdata):
                            try:
                                self.atominfo_objektlista[kolumn][rad].config(text = '{}:'.format(atomdata[i][0]))
                                self.atominfo_objektlista[kolumn+1][rad].config(text = '{}'.format(atomdata[i][1]))
                            except IndexError:
                                messagebox.showerror(title='Datafel', message="Vissa atomdata eller -egenskaper kunde inte läsas in korrekt. Kontakta utvecklaren och bifoga inputfilerna med atomdata och -egenskaper (standard: 'atomegenskaper.txt' och 'atomer.txt').\n\n{}".format(atomdata[i]), icon='warning')
                            i += 1

    def göm_atomen(self):
        self.atominfoframe.grid_forget()
        if self.menyTyp == 'lista':
            self.listframe.grid()
        else:
            self.mainframe.grid()
    
    def återgå(self):
        self.quit()

# En klass som visar poängställningen.
#    poänglistor - en lista med listor över högsta poäng för olika spellägen av datatypen dictionary

class VisaPoäng(Tk):
    def __init__(self, poänglistor):
        super().__init__()
        self.poänglistor = poänglistor
        self.title("Periodiska Systemet 178 - Poänglistor")
        self.protocol("WM_DELETE_WINDOW", self.återgå_till_huvudmenyn)

        mainframe = ttk.Frame(self, padding="3 3 12 12")
        mainframe.grid(column=0, row=0, sticky=(N, W, E, S))
        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)
        
        kolumn_id = -2
        for kolumn_nyckel, kolumn_data in self.poänglistor.items():
            kolumn_id += 4
            Label(mainframe, text = kolumn_nyckel).grid(column=kolumn_id+1, columnspan = 2, row=1, sticky=(W, E))
            Label(mainframe, text = '--------------------').grid(column=kolumn_id+1, columnspan = 2, row=2, sticky=(W, E))
            Label(mainframe, text = 'Poäng').grid(column=kolumn_id+1, row=3, sticky=(W, E))
            Label(mainframe, text = 'Datum').grid(column=kolumn_id+2, row=3, sticky=(W, E))
            Label(mainframe, text = 'Namn').grid(column=kolumn_id+3, row=3, sticky=(W, E))
            Label(mainframe, text = '     ').grid(column=kolumn_id+4, row=3, sticky=(W, E))
            rad_id = 4
            for rad in kolumn_data:
                Label(mainframe, text = '----').grid(column=kolumn_id+1, row=rad_id, sticky=(W, E))
                Label(mainframe, text = '----------').grid(column=kolumn_id+2, row=rad_id, sticky=(W, E))
                Label(mainframe, text = '----------').grid(column=kolumn_id+3, row=rad_id, sticky=(W, E))
                rad_id += 1
                Label(mainframe, text = rad[0]).grid(column=kolumn_id+1, row=rad_id, sticky=(W, E))
                Label(mainframe, text = rad[1]).grid(column=kolumn_id+2, row=rad_id, sticky=(W, E))
                Label(mainframe, text = rad[2]).grid(column=kolumn_id+3, row=rad_id, sticky=(W, E))
                rad_id += 1
            Label(mainframe, text = '----').grid(column=kolumn_id+1, row=rad_id, sticky=(W, E))
            Label(mainframe, text = '----------').grid(column=kolumn_id+2, row=rad_id, sticky=(W, E))
            Label(mainframe, text = '----------').grid(column=kolumn_id+3, row=rad_id, sticky=(W, E))
        rad_id += 1
        Button(mainframe, text="Återgå", width=25, command = self.återgå_till_huvudmenyn).grid(column=2, columnspan = kolumn_id+2, row=rad_id, sticky=(W, E))
        
    def återgå_till_huvudmenyn(self):
        self.quit()

# En klass som beskriver en huvudmeny där spelaren välja att läsa om de olika atomtyperna eller spela i ett av spellägena.

class Huvudmeny(Tk):
    def __init__(self):
        super().__init__()
        self.title("Periodiska Systemet 178")
        self.bygg_huvudmeny_användargränsnitt()
        self.handling = IntVar()
        self.protocol("WM_DELETE_WINDOW", self.avsluta_programmet)

    def bygg_huvudmeny_användargränsnitt(self):
        mainframe = ttk.Frame(self, padding="3 3 12 12")
        mainframe.grid(column=0, row=0, sticky=(N, W, E, S))
        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)
        
        Label(mainframe, text = 'Periodiska Systemet 178').grid(column=1, row=1, sticky=(W, E))
        Label(mainframe, text = 'Huvudmeny').grid(column=1, row=2, sticky=(W, E))
        Label(mainframe, text = ' ').grid(column=0, row=3)
        ttk.Separator(mainframe, orient='horizontal').grid(row=3, columnspan = 19, sticky=(W, E))
        Label(mainframe, text = 'Välkommen till spelet som ska\nlära dig periodiska systemet!').grid(column=1, row=4, sticky=(W, E))
        Label(mainframe, text = ' ').grid(column=0, row=5)
        ttk.Separator(mainframe, orient='horizontal').grid(row=5, columnspan = 19, sticky=(W, E))
        Button(mainframe, text="Bygg periodiska systemet", width=25, command = self.bygg_periodiska_systemet).grid(column=1, row=6, sticky=(W, E))
        Button(mainframe, text="Frågesport", width=25, command = self.starta_frågesport).grid(column=1, row=7, sticky=(W, E))
        Button(mainframe, text="Läs om Atomer", width=25, command = self.läs_om_atomer).grid(column=1, row=8, sticky=(W, E))
        Button(mainframe, text="Visa poäng", width=25, command = self.visa_poäng).grid(column=1, row=9, sticky=(W, E))
        Label(mainframe, text = ' ').grid(column=0, row=10)
        ttk.Separator(mainframe, orient='horizontal').grid(row=10, columnspan = 19, sticky=(W, E))
        Label(mainframe, text = '⚛', font=("Arial", 72)).grid(column=1, row=11, sticky=(W, E))
        Label(mainframe, text = ' ').grid(column=0, row=12)
        ttk.Separator(mainframe, orient='horizontal').grid(row=12, columnspan = 19, sticky=(W, E))
        Button(mainframe, text="Avsluta", width=25, command = self.avsluta_programmet).grid(column=1, row=13, sticky=(W, E))

    def bygg_periodiska_systemet(self):
        self.handling = 1
        self.quit()

    def starta_frågesport(self):
        self.handling = 2
        self.quit()

    def läs_om_atomer(self):
        self.handling = 3
        self.quit()

    def visa_poäng(self):
        self.handling = 4
        self.quit()
    
    def avsluta_programmet(self):
        if self.visa_bekräfta_avslut_meddelande():
            self.handling = 0
            self.quit()
    
    def visa_bekräfta_avslut_meddelande(self):
        return messagebox.askyesno(title='Bekräfta avslut', message='Vill du verkligen avsluta?', icon='warning')


def ladda_atomdata(filnamn1,filnamn2):
    atomdata = {}
    atomegenskaper_rådata = {}
    try:
        with open(filnamn1, 'r') as f:
            for r in f.read().split('\n'):
                if not r.startswith('#'):
                    data = r.split(';')
                    atomdata[data[0]] = {'Grundämne' : data[1], 'Atomnummer' : data[2], 'Atommassa (u)' : data[3]}
    except Exception:
        return {}, {}, 'Fel vid inläsning av {}!'.format(filnamn1), False
    try:
        with open(filnamn2, 'r') as f:
            for egenskapsgrupp in f.read().split('## '):
                egenskaper = egenskapsgrupp.split('\n')
                if len(egenskaper[0]) > 0:
                    atomegenskaper_rådata[egenskaper[0]] = {}
                    for i in range(1,len(egenskaper)):
                        if not egenskaper[i].startswith('#'):
                            if len(egenskaper[i].strip()) > 0:
                                egenskap = egenskaper[i].split(';')
                                atomegenskaper_rådata[egenskaper[0]][egenskap[0]] = egenskap[1].split(',')
        atomegenskaper = {}
        for i in range(1,len(atomdata.keys())+1):
            atomegenskaper[str(i)] = {}
            for atomegenskap, atomegenskapsdata in atomegenskaper_rådata.items():
                for egenskapsvärde, atomids in atomegenskapsdata.items():
                    if str(i) in atomids:
                        atomegenskaper[str(i)][atomegenskap] = egenskapsvärde
            if len(atomegenskaper[str(i)].keys()) < len(atomegenskaper_rådata.keys()):
                return {}, {}, 'Fel vid inläsning av {}, atom med atomnummer {} saknar {} atomegenskap(er)!'.format(filnamn2,i,len(atomegenskaper_rådata.keys())-len(atomegenskaper[str(i)].keys())), False
    except Exception:
        return {}, {}, 'Fel vid inläsning av {}!'.format(filnamn2), False
    return atomdata, atomegenskaper, '', True

def bygg_atomer(atomdata, atomegenskaper):
    atomer = []
    for tecken, atominformation in atomdata.items():
        atomer.append(Atom(tecken=tecken, namn=atominformation['Grundämne'], atomnummer=atominformation['Atomnummer'], atommassa=atominformation['Atommassa (u)'], atomegenskaper=atomegenskaper[atominformation['Atomnummer']]))
    return atomer

def bestäm_högsta_informationsmängd(atomer):
    högsta_informationsmängd = 0
    for a in atomer:
        informationsmängd = len(str(a).split('\n'))
        if informationsmängd > högsta_informationsmängd:
            högsta_informationsmängd = informationsmängd
    return högsta_informationsmängd

def ladda_poänghistorik(filnamn):
    try:
        poänghistorik = {}
        with open(filnamn, 'r') as f:
            for poänghistorikstyp in f.read().split('#'):
                if len(poänghistorikstyp.split('\n')[0]) > 0:
                    poänghistorik[poänghistorikstyp.split('\n')[0]] = []
                    for poängrad in poänghistorikstyp.split('\n')[1:11]:
                        poänghistorik[poänghistorikstyp.split('\n')[0]].append([poängrad.split(',')[0],datetime.strptime(poängrad.split(',')[1], "%Y-%m-%d %H:%M:%S"),poängrad.split(',')[2]])
    except Exception:
        return {}, 'Fel vid inläsning av {}!'.format(filnamn), False
    return poänghistorik, '', True

def spara_poänghistorik(filnamn, poänglistor):
    with open(filnamn,'w') as f:
        for poänghistorikstyp, poänghistorik in poänglistor.items():
            f.write('#{}\n'.format(poänghistorikstyp))
            for poäng in poänghistorik:
                f.write('{},{},{}\n'.format(poäng[0],poäng[1].strftime("%Y-%m-%d %H:%M:%S"),poäng[2]))

def huvudfunktion(atomfil='atomer.txt',atomegenskapsfil='atomegenskaper.txt',poänghistoriksfil='poanglista.txt'):
    print('\n   Välkommen till programmet för att lära dig det periodiska systemet!\n\nStartar upp grafiskt användargränsnitt.\n')
    atomdata, atomegenskaper, felmeddelande1, laddning1_OK = ladda_atomdata(atomfil,atomegenskapsfil)
    if laddning1_OK:
        print('Atomerna har laddats.')
        atomer = bygg_atomer(atomdata, atomegenskaper)
        print('Atomerna har byggts.')
        poänghistorik, felmeddelande2, laddning2_OK = ladda_poänghistorik(poänghistoriksfil)
        # laddning2_OK = False
        if laddning2_OK:
            print('Poänglistor importerade.')
            max_atominfo = bestäm_högsta_informationsmängd(atomer)
            # Här kör själva programmet i form av mini-program
            program = Huvudmeny()
            while True:
                program.mainloop()
                program.withdraw()
                handling = program.handling
                if handling == 0:
                    break
                elif handling == 1:
                    program1 = ByggPeriodiskaSystemet(poänglista = poänghistorik, atomer=atomer)
                    program1.mainloop()
                    poänghistorik = program1.poänglista
                    program1.withdraw()
                    program.deiconify()
                elif handling == 2:
                    program2 = Frågesport(poänglista = poänghistorik, atomer=atomer)
                    program2.mainloop()
                    poänghistorik = program2.poänglista
                    program2.withdraw()
                    program.deiconify()
                elif handling == 3:
                    program3 = Läsning(atomer=atomer, max_atominfo=max_atominfo)
                    program3.mainloop()
                    program3.withdraw()
                    program.deiconify()
                elif handling == 4:
                    program4 = VisaPoäng(poänglistor=poänghistorik)
                    program4.mainloop()
                    program4.withdraw()
                    program.deiconify()
            # Här avslutades programmet
            print('Programmet avslutas.')
            spara_poänghistorik(poänghistoriksfil, poänghistorik)
            print('Poänglistorna har sparats.')
        else:
            print(felmeddelande2)
    else:
        print(felmeddelande1)
    print('\nProgrammet har avslutats. Tack för att du övar på periodiska system!\n')

if __name__ == "__main__":
    huvudfunktion()
```
