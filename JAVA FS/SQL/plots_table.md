CREATE DATABASE realestate_db;
USE realestate_db;

CREATE TABLE plots (
    id INT AUTO_INCREMENT PRIMARY KEY,
    plot_no VARCHAR(20),
    facing VARCHAR(10)
);

INSERT INTO plots (plot_no, facing)
VALUES ('P-101', 'E'), ('P-102', 'N');
