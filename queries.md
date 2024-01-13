# Queries do desafio
-- selecionar todos os veiculos<br/>

SELECT * FROM veiculo;<br/>

-- Selecionar o nome e endereço dos mecânicos<br/>

SELECT nome, lastnome, endereço FROM mecanico;<br/>

-- Filtros com WHERE Selecionar clientes que possuem carros modelo 'Civic'<br/>

SELECT c.nome as Cliente, c.lastnome as sobrenome, v.carro<br/>
FROM cliente c<br/>
INNER JOIN veiculo v ON c.cli_idveiculo = v.idveiculo<br/>
WHERE v.modelo = 'Civic';<br/>

-- Selecionar serviços com valor superior a 150.00<br/>

SELECT * FROM os_serviço<br/>
WHERE valor > 150.00;<br/>

-- Calcular a idade dos veículos<br/>

SELECT carro, modelo, YEAR(NOW()) - YEAR(ano) AS idade<br/>
FROM veiculo;<br/>

-- Criar um campo para indicar se a peça é cara ou barata (considerando valor > 50.00 como caro)<br/>

SELECT nome, valor,<br/>
       CASE WHEN valor > 50.00 THEN 'caro' ELSE 'barato' END AS preço_da_peça<br/>
FROM peca;<br/>

-- Listar clientes que ainda não realizaram nenhum serviço<br/>

SELECT c.nome, c.lastnome<br/>
FROM cliente c<br/>
LEFT JOIN os_serviço os ON c.idcliente = os.serv_idpedidocliente<br/>
WHERE os.idos_serviço IS NULL;<br/>

-- Calcular o valor total de peças utilizadas em cada serviço<br/>

SELECT s.idserviço, SUM(p.valor) as total_peças<br/>
FROM serviço s<br/>
INNER JOIN peca p ON s.peca_idpecas = p.idpeca<br/>
GROUP BY s.idserviço;<br/>

-- Recuperar os serviços que estão aguardando aprovação por mais de 3 dias<br/>

SELECT *<br/>
FROM os_serviço<br/>
WHERE aprovação_cli = 'aguardando' AND DATEDIFF(NOW(), dataemissao) > 3;<br/>
