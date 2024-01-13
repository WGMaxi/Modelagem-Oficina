# Modelagem-Oficina
O Código usado para criar o banco de dados da oficna baseado no Diagrama EER.<br/>
-- criação do banco de dados para o cenário de oficina mecanica<br/>


create database oficina;<br/>

use oficina;<br/>

    -- criar tabela veiculo<br/>
   
create table veiculo(<br/>
		idveiculo int auto_increment primary key,<br/>
        carro varchar(45) not null,<br/>
        modelo varchar(11)not null,<br/>
        ano date not null<br/>
); <br/>       

	-- criar tabela cliente <br/>
    
create table cliente(<br/>
		idcliente int auto_increment primary key,<br/>
        cli_idveiculo int,<br/>
        nome varchar(45) not null,<br/>
        lastnome varchar(45) not null,<br/>
        cpf char(14) not null,<br/>
        cnh char(9) not null,<br/>
        endereço varchar(45) not null,<br/>
        constraint unique_cpf_client unique(cpf),<br/>
        constraint unique_cnh_client unique(cnh),<br/>
        constraint fk_veiculo_cliente foreign key (cli_idveiculo) references veiculo(idveiculo)<br/>
);<br/>

-- criar tabela mecanico<br/>

create table mecanico(<br/>
		idmecanico int auto_increment primary key,<br/>
        nome varchar(45) not null,<br/>
        lastnome varchar(45) not null,<br/>
        endereço varchar(45) not null,<br/>
        especialidade varchar(45) not null  <br/>     
);<br/>

	-- criar tabela os_serviço<br/>

create table os_serviço(<br/>
		idos_serviço int auto_increment primary key,<br/>
        serv_idpedidocliente int,<br/>
        serv_idmecanico int,<br/>
        os_status enum('cancelado','entregue','em processamento') not null default 'em processamento',<br/>
        aprovação_cli enum('cancelado','aprovado','aguardando') not null default 'aguardando',<br/>
        descrição varchar(255),<br/>
        dataemissao date not null,	<br/>
        datafim date,	<br/>
        valor decimal (10,2),<br/>
        constraint fk_os_cliente foreign key (serv_idpedidocliente) references cliente(idcliente),<br/>
        constraint fk_os_mecanico foreign key (serv_idmecanico) references mecanico(idmecanico)<br/>
	); <br/>

	-- criar tabela peças<br/>
    
        create table peca(<br/>
		idpeca int auto_increment primary key,<br/>
        nome varchar(255),<br/>
        valor  decimal (10,2),<br/>
        descricao varchar(255)<br/>
); <br/>

	-- criar tabela mão de obra<br/>
    
        create table mobra(<br/>
		imobra int auto_increment primary key,<br/>
        tiposerv varchar(255),<br/>
        valor  decimal (10,2),<br/>
        descricao varchar(255)<br/>
); <br/>


	-- criar tabela serviços<br/>
    
        create table serviço(<br/>
		idserviço int auto_increment primary key,<br/>
        peca_idpecas int,<br/>
        mobra_idmobra int,<br/>
        constraint fk_servic_peca foreign key (peca_idpecas) references peca(idpeca),<br/>
        constraint fk_servic_mobra foreign key (mobra_idmobra) references mobra(imobra)<br/>
); <br/>

	-- criar tabela serviços pra compor os<br/>
    
        create table serviço_compor_os(<br/>
		comp_os_idserv int,<br/>
        comp_os_idos int,<br/>
        constraint fk_servcomp_serv foreign key (comp_os_idserv) references serviço(idserviço),<br/>
        constraint fk_servcomp_ordem foreign key (comp_os_idos) references os_serviço(idos_serviço) <br/>
);  <br/>
