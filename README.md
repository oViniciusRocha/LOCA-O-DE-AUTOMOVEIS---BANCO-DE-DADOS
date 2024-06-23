Este exercicio consiste em um banco de dados comum, porém normalizado, isto é, segue as formas normais 1FN, 2FN e 3FN.

Este banco de dados é ultilizado em um sistema de locação de automoveis onde há 3 tabelas,
uma para armazenar veículos, uma para clientes e outra para armazenar os dados de locação.

------------------------------CRIAÇÃO DAS TABELAS-----------------------------------------

create table veiculo(

	placa	varchar(8) primary key,
    veiculo	varchar(120),
    cor		varchar(60)
);

create table cliente(

    cpf			varchar(14) primary key,
	cliente		varchar(120),
    nascimento	date
);

create table locação(

	id		int	primary key auto_increment,
    diaria		double,
    dias		int,
	total		double generated always as (diaria*dias) stored,
    idcliente	varchar(14),
    idveiculo	varchar(8),
    
    foreign key (idcliente) references cliente(cpf),
    foreign key (idveiculo) references veiculo(placa)
);

Com as tabelas criadas o próximo passo é  a inserção dos clientes veiculos e as locações que cada cliente realizou


--------------------------------------------INSERÇÕES----------------------------------------------------

insert into veiculo (veiculo, cor, placa) values

('Fusca', 'Preto', 'WER-3456'),

('Variante', 'Rosa', 'FDS-8384'),

('Comodoro', 'Preto', 'CVB-9933'),

('Deloriam', 'Azul', 'FGH-2256'),

('Brasília', 'Amarela', 'DDI-3948');

----------------------------------------------------------

insert into cliente (cliente, cpf, nascimento) values

('Ariano Suassuna', '123.456.789-01', '1984-05-21'),

('Grace Hopper', '543.765.987-23', '1965-04-29'),

('Richard Feynman', '987.654.231-90', '1954-10-12'),

('Edgar Poe', '432.765.678-67', '1964-12-14'),

('Gordon Freeman', '927.384.284-44', '1967-11-26');

-----------------------------------------------------------

insert into locação (diaria, dias, idcliente, idveiculo) values

(30.00, 3, '123.456.789-01', 'WER-3456'),

(60.00, 7, '543.765.987-23', 'FDS-8384'),

(20.00, 1, '987.654.231-90', 'CVB-9933'),

(80.00, 3, '432.765.678-67', 'FGH-2256'),

(45.00, 7, '927.384.284-44', 'DDI-3948');

----------------------------------------------------------

Feito isso ja é possivel consultar as locações de cada cliente analizando as tabélas, porém para ficar mais facil,
dentro do próprio banco de dados é criado uma View(vizualização) que facilita a vizualização das locações; Para a
criação desse view foi ultilizado inner join que permite a vizualização das relações das tabelas

-----------------------------------------------VIEW DAS LOCAÇÕES----------------------------------------------------

create view LocaçãoId as

select l.id,l.total,c.cliente,v.veiculo from locação l 

inner join cliente c on l.idcliente = c.cpf

inner join veiculo v on l.idveiculo = v.placa;

select * from LocaçãoId;


com esse comando select o retorno deste view é esse:

+----+--------+-----------------+-----------+

| id | total  | cliente         | veiculo   |


|  1 |  90.00 | Ariano Suassuna | Fusca     |

|  2 | 420.00 | Grace Hopper    | Variante  |

|  3 |  20.00 | Richard Feynman | Comodoro  |

|  4 | 240.00 | Edgar Poe       | Deloriam  |

|  5 | 315.00 | Gordon Freeman  | Brasília  |

+----+--------+-----------------+-----------+
