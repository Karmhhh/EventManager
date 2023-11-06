drop type "CONTACTS";
drop type "RANGEDATE";

DROP TABLE "Persona",
DROP TABLE "Location";
DROP TABLE "Catalogo_Prodotti";
DROP TABLE "Prodotto";
DROP TABLE "Programma_Esibizioni";
DROP TABLE "Esibizione";
DROP TABLE "Artisti";
DROP TABLE "Esib_ProgEsib";
DROP TABLE "Evento";
DROP TABLE "Stand";
DROP TABLE "Biglietto";
DROP TABLE "Staff";
DROP TABLE "Lavoratore";
DROP TABLE "Turno";



create type contacts as object (
    telefono varchar(20),
    email varchar(20),
    Indirizzo_Residenza varchar(200)
);

CREATE type rangedate as object ( 
 	inizio date, 
   	fine date 
);  

create table Persona(
    Cf_Pk varchar(20) not null constraint Cf_pk primary key,
    età int not null,
    contatti contacts not null,
    Nominativo varchar(100) not null
    
);

create table Location ( 
    Location_Pk varchar(100) not null constraint location_pk primary key, 
    Indirizzo varchar(100) not null, 
    MaxPosti number(1) not null 
);

create table Catalogo_Prodotti( 
    Codice_Catalogo_Pk varchar(20) not null constraint codice_catalogo_pk primary key, 
    Descrizione varchar(200) not null, 
    Anno_Catalogo date not null
   
);

create table Prodotto( 
    Codice_Prodotto_Pk varchar(20) not null constraint codice_prodotto_pk primary key, 
    Codice_Catalogo_fk  varchar(16) not null, 
    constraint codice_catalogo_fk Foreign Key (Codice_Catalogo_fk) REFERENCES Catalogo_Prodotti(Codice_Catalogo_Pk) ,
    Prezzo float not null,
    Nome varchar(200) not null, 
	Categoria  varchar(20)
);

create table Programma_Esibizioni( 
    Codice_Programma_Esibizioni_Pk varchar(10) not null constraint codice_programma_esibizioni_pk primary key, 
    Descrizione varchar(20) not null, 
	Max_Esibizioni int
);

create table Esibizione( 
 Codice_Esibizione_Pk varchar(10) not null constraint codice_esibizione_pk primary key, 
    Categoria varchar(20) not null
	
);

create table Artisti( 
 Matricola_Pk varchar(10) not null constraint  matricola_pk primary key, 
    Nome_Arte varchar(20) not null,
    Codice_Esibizione_fk varchar(20) not null,
    constraint  codice_esibizione_fk  foreign key (Codice_Esibizione_fk) REFERENCES Esibizione(Codice_Esibizione_Pk),
	contatti contacts
);

create table Esib_ProgEsib( 
 Codice_Esib_ProgEsib_Pk varchar(10) not null constraint codice_esib_progEsib_pk primary key, 
    Codice_ProgrammaEsibizioni_fk varchar(20) not null,
    constraint  codice_programmaEsibizioni_fk  foreign key (Codice_ProgrammaEsibizioni_fk) REFERENCES Programma_Esibizioni( Codice_Programma_Esibizioni_Pk ),
    Codice_Esibizione_fk varchar(20) not null,
    constraint  codice_esibizione_rfk  foreign key (Codice_Esibizione_fk) REFERENCES Esibizione(Codice_Esibizione_Pk)
	
);

create table Evento(
    Codice_Evento_Pk varchar(20) not null constraint CodiceEvento_pk primary key,
    Nome varchar(20) not null,
    Descrizione varchar(100) not null,
    Location varchar(20) not null, 
    constraint Location_Fk Foreign Key (Location) REFERENCES Location(Location_pk), 
    Cod_ProgEsib_fk varchar(20) not null, 
    constraint cod_progEsib_fk Foreign key (Cod_ProgEsib_fk) REFERENCES Programma_Esibizioni( Codice_Programma_Esibizioni_Pk),
    Dates rangedate
);


create table Stand( 
    Codice_Stand_Pk varchar(10) not null constraint codice_stand_pk primary key, 
    Codice_Evento_sfk varchar(20) not null, 
    constraint codice_evento_sfk Foreign key (Codice_Evento_sfk) REFERENCES Evento(Codice_Evento_Pk ), 
    Codice_CatalogoProdotti_sfk varchar(20) not null, 
    constraint codice_catalogoProdotti_sfk Foreign key (Codice_CatalogoProdotti_sfk ) REFERENCES Catalogo_Prodotti(Codice_Catalogo_Pk),  
    Nome varchar(20),
    Descrizione varchar(20)

);

create table Biglietto( 
    Codice_Biglietto_Pk varchar(10) not null constraint codice_biglietto_pk primary key, 
    Codice_Evento_bfk varchar(20) not null, 
    constraint codice_evento_bfk Foreign key (Codice_Evento_bfk) REFERENCES Evento(Codice_Evento_Pk ), 
    Cod_Fisc_FK varchar(16) not null, 
    constraint cod_fisc_fk Foreign Key (Cod_Fisc_FK) REFERENCES Persona(Cf_Pk ) ,
    data_acquisto date,
    scandeza_validità date

);

create table Staff( 
    Codice_Staff_Pk varchar(100) not null constraint codice_staff_pk primary key, 
    Numero_Componenti int not null,
    Descrizione varchar(100)  not null,
    Max_Componenti int not null,
 Codice_Evento_fk varchar(20) ,constraint codice_evento_fk Foreign Key (Codice_Evento_fk) REFERENCES Evento( Codice_Evento_Pk) 
);

create table Lavoratore( 
    Cf_Lavoratore_Pk varchar(20) not null constraint cf_lavoratore_pk primary key, 
    Codice_Staff_fk  varchar(16) not null, 
    constraint codice_staff_fk Foreign Key (Codice_Staff_fk) REFERENCES Staff(Codice_Staff_Pk) ,
    Eta int not null,
    contatti contacts,
Ruolo  varchar(20)
);

create table Turno (
    Codice_Turno_Pk varchar(40) not null constraint codice_turno_pk primary key,
    dates rangedate not null,
    
    Giorno varchar(40) not null,
    Cf_Lavoratore_fk varchar(40) not null, constraint cf_lavoratore_fk  foreign key (Cf_Lavoratore_fk) references Lavoratore(Cf_Lavoratore_Pk)
);




