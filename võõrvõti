-- phpMyAdmin SQL Dump
-- version 5.2.2
-- https://www.phpmyadmin.net/

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";

-- --------------------------------------------------------
--
-- Table structure for table `users`
--
CREATE TABLE `users` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'User ID',
  `name` varchar(100) NOT NULL COMMENT 'Full name',
  `email` varchar(150) NOT NULL COMMENT 'Email address',
  `password` varchar(255) NOT NULL COMMENT 'Hashed password',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp() COMMENT 'Account created',
  PRIMARY KEY (`id`),
  UNIQUE KEY `email` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `hosts`
--
CREATE TABLE `hosts` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'Host ID',
  `user_id` int(11) UNSIGNED NOT NULL COMMENT 'Seos kasutajaga',
  `location` varchar(255) DEFAULT NULL COMMENT 'Asukoht',
  `is_superhost` tinyint(1) NOT NULL DEFAULT 0 COMMENT 'Superhost? 0/1',
  `response_rate` decimal(5,2) DEFAULT NULL CHECK (`response_rate` BETWEEN 0 AND 100) COMMENT 'Vastamise protsent',
  `since` date DEFAULT NULL COMMENT 'Liitumise kuupäev',
  `profile_pic` varchar(255) DEFAULT NULL COMMENT 'Profiilipilt URL',
  `phone_verified` tinyint(1) NOT NULL DEFAULT 0 COMMENT 'Telefon kinnitatud',
  `about` text DEFAULT NULL COMMENT 'Kirjeldus',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `fk_hosts_user` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `room_types`
--
CREATE TABLE `room_types` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL UNIQUE,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- --------------------------------------------------------
--
-- Table structure for table `countries`
--
CREATE TABLE `countries` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(100) NOT NULL UNIQUE,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- --------------------------------------------------------
--
-- Table structure for table `cities`
--
CREATE TABLE `cities` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `country_id` int(11) UNSIGNED NOT NULL,
  `name` varchar(100) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_city_in_country` (`country_id`, `name`),
  CONSTRAINT `fk_city_country` FOREIGN KEY (`country_id`) REFERENCES `countries` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- --------------------------------------------------------
--
-- Table structure for table `listings`
--
CREATE TABLE `listings` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'Unikaalne ID',
  `name` varchar(255) NOT NULL COMMENT 'Majutuse nimi',
  `host_id` int(11) UNSIGNED NOT NULL COMMENT 'Host ID',
  `city_id` int(11) UNSIGNED NOT NULL COMMENT 'Linna ID',
  `price` decimal(10,2) NOT NULL CHECK (`price` >= 0) COMMENT 'Hind öö kohta',
  `room_type_id` int(11) UNSIGNED NOT NULL COMMENT 'Toatüübi ID',
  `accommodates` int(11) NOT NULL CHECK (`accommodates` >= 1) COMMENT 'Max külalised',
  `bedrooms` int(11) DEFAULT NULL COMMENT 'Magamistubade arv',
  `beds` int(11) DEFAULT NULL COMMENT 'Voodite arv',
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_listing_name` (`name`),
  KEY `host_id` (`host_id`),
  KEY `city_id` (`city_id`),
  KEY `room_type_id` (`room_type_id`),
  CONSTRAINT `fk_listings_host` FOREIGN KEY (`host_id`) REFERENCES `hosts` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_listings_city` FOREIGN KEY (`city_id`) REFERENCES `cities` (`id`),
  CONSTRAINT `fk_listings_room_type` FOREIGN KEY (`room_type_id`) REFERENCES `room_types` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `reviews`
--
CREATE TABLE `reviews` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'Arvustuse ID',
  `listing_id` int(11) UNSIGNED NOT NULL COMMENT 'Seos listinguga',
  `reviewer_id` int(11) UNSIGNED DEFAULT NULL COMMENT 'Arvustaja ID, NULL = anonüümne',
  `reviewed_at` date DEFAULT NULL COMMENT 'Arvustuse kuupäev',
  `rating` decimal(3,2) DEFAULT NULL CHECK (`rating` BETWEEN 0 AND 5) COMMENT 'Hinnang',
  `comment` text DEFAULT NULL COMMENT 'Kommentaar',
  `accuracy` decimal(3,2) DEFAULT NULL CHECK (`accuracy` BETWEEN 0 AND 5) COMMENT 'Täpsus',
  `cleanliness` decimal(3,2) DEFAULT NULL CHECK (`cleanliness` BETWEEN 0 AND 5) COMMENT 'Puhtus',
  PRIMARY KEY (`id`),
  KEY `listing_id` (`listing_id`),
  KEY `reviewer_id` (`reviewer_id`),
  CONSTRAINT `fk_reviews_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_reviews_user` FOREIGN KEY (`reviewer_id`) REFERENCES `users` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `bookings`
--
CREATE TABLE `bookings` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'Broneeringu ID',
  `user_id` int(11) UNSIGNED NOT NULL COMMENT 'Külalise ID',
  `listing_id` int(11) UNSIGNED NOT NULL COMMENT 'Majutuse ID',
  `check_in` date NOT NULL,
  `check_out` date NOT NULL,
  `guests` int(11) NOT NULL CHECK (`guests` >= 1) COMMENT 'Külaliste arv',
  `status` enum('pending','confirmed','cancelled') NOT NULL DEFAULT 'pending',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  KEY `listing_id` (`listing_id`),
  CONSTRAINT `fk_bookings_user` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_bookings_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings` (`id`) ON DELETE CASCADE,
  CONSTRAINT `chk_booking_dates` CHECK (`check_out` > `check_in`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `amenities`
--
CREATE TABLE `amenities` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'Amenity ID',
  `name` varchar(100) NOT NULL UNIQUE COMMENT 'Mugavuse nimi',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

-- --------------------------------------------------------
--
-- Table structure for table `listing_amenities`
--
CREATE TABLE `listing_amenities` (
  `listing_id` int(11) UNSIGNED NOT NULL,
  `amenity_id` int(11) UNSIGNED NOT NULL,
  PRIMARY KEY (`listing_id`, `amenity_id`),
  CONSTRAINT `fk_la_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_la_amenity` FOREIGN KEY (`amenity_id`) REFERENCES `amenities` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_uca1400_ai_ci;

COMMIT;



-- Lisame võõrvõtmed ja indeksid, et tagada andmete terviklikkus ja päringute optimeerimine

-- --------------------------------------------------------
-- INDEXID JA VÕÕRVÕTMED listings
-- --------------------------------------------------------
ALTER TABLE `listings`
  ADD CONSTRAINT `fk_listings_host` FOREIGN KEY (`host_id`) REFERENCES `hosts`(`id`) ON DELETE CASCADE,
  ADD CONSTRAINT `fk_listings_city` FOREIGN KEY (`city_id`) REFERENCES `cities`(`id`),
  ADD CONSTRAINT `fk_listings_room_type` FOREIGN KEY (`room_type_id`) REFERENCES `room_types`(`id`);

CREATE INDEX `idx_listings_host_id` ON `listings`(`host_id`);
CREATE INDEX `idx_listings_city_id` ON `listings`(`city_id`);
CREATE INDEX `idx_listings_room_type_id` ON `listings`(`room_type_id`);

-- --------------------------------------------------------
-- INDEXID JA VÕÕRVÕTMED reviews
-- --------------------------------------------------------
ALTER TABLE `reviews`
  ADD CONSTRAINT `fk_reviews_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings`(`id`) ON DELETE CASCADE,
  ADD CONSTRAINT `fk_reviews_user` FOREIGN KEY (`reviewer_id`) REFERENCES `users`(`id`);

CREATE INDEX `idx_reviews_listing_id` ON `reviews`(`listing_id`);
CREATE INDEX `idx_reviews_reviewer_id` ON `reviews`(`reviewer_id`);

-- --------------------------------------------------------
-- INDEXID JA VÕÕRVÕTMED bookings
-- --------------------------------------------------------
ALTER TABLE `bookings`
  ADD CONSTRAINT `fk_bookings_user` FOREIGN KEY (`user_id`) REFERENCES `users`(`id`) ON DELETE CASCADE,
  ADD CONSTRAINT `fk_bookings_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings`(`id`) ON DELETE CASCADE;

CREATE INDEX `idx_bookings_user_id` ON `bookings`(`user_id`);
CREATE INDEX `idx_bookings_listing_id` ON `bookings`(`listing_id`);

-- --------------------------------------------------------
-- INDEXID JA VÕÕRVÕTMED listing_amenities
-- --------------------------------------------------------
ALTER TABLE `listing_amenities`
  ADD CONSTRAINT `fk_la_listing` FOREIGN KEY (`listing_id`) REFERENCES `listings`(`id`) ON DELETE CASCADE,
  ADD CONSTRAINT `fk_la_amenity` FOREIGN KEY (`amenity_id`) REFERENCES `amenities`(`id`) ON DELETE CASCADE;

CREATE INDEX `idx_la_listing_id` ON `listing_amenities`(`listing_id`);
CREATE INDEX `idx_la_amenity_id` ON `listing_amenities`(`amenity_id`);
