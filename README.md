-- Criação do banco de dados
CREATE DATABASE IF NOT EXISTS cinetech CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE cinetech;

-- Tabela de gêneros
CREATE TABLE IF NOT EXISTS generos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    descricao TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Tabela de filmes
CREATE TABLE IF NOT EXISTS filmes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    sinopse TEXT NOT NULL,
    capa_url VARCHAR(255) NOT NULL,
    trailer_url VARCHAR(255) NOT NULL,
    data_lancamento DATE NOT NULL,
    duracao INT NOT NULL COMMENT 'Duração em minutos',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Tabela de relacionamento muitos-para-muitos entre filmes e gêneros
CREATE TABLE IF NOT EXISTS filme_genero (
    filme_id INT NOT NULL,
    genero_id INT NOT NULL,
    PRIMARY KEY (filme_id, genero_id),
    FOREIGN KEY (filme_id) REFERENCES filmes(id) ON DELETE CASCADE,
    FOREIGN KEY (genero_id) REFERENCES generos(id) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Inserir alguns gêneros iniciais
INSERT INTO generos (nome, descricao) VALUES
('Ação', 'Filmes com muita ação e aventura'),
('Comédia', 'Filmes humorísticos'),
('Drama', 'Filmes com conteúdo dramático'),
('Ficção Científica', 'Filmes com temas futuristas e científicos'),
('Terror', 'Filmes assustadores');

-- Criar índice para melhorar buscas
CREATE FULLTEXT INDEX idx_filme_search ON filmes(titulo, sinopse);
