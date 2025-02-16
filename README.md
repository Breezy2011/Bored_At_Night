# Bored_At_Night
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bored at Night - Movie Finder</title>
    <!-- Add Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            margin: 0;
            padding: 0;
            min-height: 100vh;
            background: #000000;
            color: #fff;
            font-size: 16px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
        }

        h1, .movie-title {
            font-family: 'Outfit', sans-serif;
            font-weight: 700;
            font-size: 3.2em;
            margin-bottom: 15px;
            color: #fff;
            text-shadow: 0 0 20px rgba(0, 255, 245, 0.5);
            letter-spacing: 2px;
        }

        .subtitle {
            font-family: 'Poppins', sans-serif;
            font-weight: 400;
            font-size: 1.4em;
            color: rgba(255, 255, 255, 0.9);
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            margin-bottom: 30px;
        }

        .quiz-container {
            background: rgba(20, 20, 20, 0.7);
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(255, 255, 255, 0.2);
            position: relative;
            overflow: hidden;
            min-height: 300px;
        }

        .question {
            display: none;
            opacity: 0;
            transform: translateX(50px);
            position: absolute;
            width: 100%;
            transition: all 0.5s ease;
        }

        .question.active {
            display: block;
            opacity: 1;
            transform: translateX(0);
            position: relative;
            animation: slideIn 0.5s ease forwards;
        }

        .question.exit {
            display: block;
            animation: slideOut 0.5s ease forwards;
        }

        @keyframes slideIn {
            0% {
                opacity: 0;
                transform: translateX(100px);
            }
            100% {
                opacity: 1;
                transform: translateX(0);
            }
        }

        @keyframes slideOut {
            0% {
                opacity: 1;
                transform: translateX(0);
            }
            100% {
                opacity: 0;
                transform: translateX(-100px);
            }
        }

        .question h2 {
            font-family: 'Outfit', sans-serif;
            font-weight: 600;
            color: #fff;
            margin-bottom: 30px;
            font-size: 2em;
            text-shadow: 0 0 20px rgba(0, 255, 245, 0.5);
            letter-spacing: 2px;
        }

        .options {
            display: grid;
            gap: 20px;
            padding: 10px;
        }

        .option {
            font-family: 'Poppins', sans-serif;
            font-weight: 400;
            background: rgba(20, 20, 20, 0.8);
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 20px 25px;
            font-size: 1.3em;
            text-align: center;
            transform: scale(0.95);
            opacity: 0;
            animation: optionAppear 0.5s ease forwards;
        }

        .option:hover {
            background: rgba(0, 255, 245, 0.15);
            border-color: #00fff5;
            transform: translateY(-3px);
            box-shadow: 0 5px 20px rgba(0, 255, 245, 0.3);
        }

        @keyframes optionAppear {
            from {
                transform: scale(0.95);
                opacity: 0;
            }
            to {
                transform: scale(1);
                opacity: 1;
            }
        }

        .option:nth-child(1) { animation-delay: 0.1s; }
        .option:nth-child(2) { animation-delay: 0.2s; }
        .option:nth-child(3) { animation-delay: 0.3s; }
        .option:nth-child(4) { animation-delay: 0.4s; }
        .option:nth-child(5) { animation-delay: 0.5s; }

        #results {
            display: none;
            animation: fadeIn 0.5s ease-out;
        }

        .movie-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 25px;
            margin-top: 20px;
        }

        .movie-card {
            background: rgba(20, 20, 20, 0.7);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            transition: transform 0.3s ease;
            height: 100%;
            display: flex;
            flex-direction: column;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .movie-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 255, 245, 0.2);
            background: rgba(20, 20, 20, 0.9);
        }

        .movie-poster {
            width: 100%;
            height: 250px;
            object-fit: cover;
            border-radius: 5px;
            margin-bottom: 10px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
        }

        .movie-title {
            font-size: 1.3em;
            margin: 12px 0;
            color: #00fff5;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .movie-genre {
            font-family: 'Poppins', sans-serif;
            font-weight: 400;
            color: rgba(255, 255, 255, 0.8);
            font-size: 1.1em;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .progress-bar {
            width: 100%;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            margin: 20px 0;
            border-radius: 2px;
        }

        .progress {
            width: 0%;
            height: 100%;
            background: #00fff5;
            border-radius: 2px;
            transition: width 0.3s ease;
        }

        .loading {
            display: none;
            text-align: center;
            margin: 20px 0;
        }

        .loading-text {
            font-family: 'Outfit', sans-serif;
            font-weight: 600;
            color: #00fff5;
            font-size: 1.4em;
            margin-bottom: 15px;
        }

        .dots {
            display: flex;
            justify-content: center;
            gap: 8px;
        }

        .dot {
            width: 8px;
            height: 8px;
            background: #00fff5;
            border-radius: 50%;
            animation: bounce 0.5s infinite alternate;
        }

        .dot:nth-child(2) { animation-delay: 0.2s; }
        .dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes bounce {
            to { transform: translateY(-8px); }
        }

        .platform-links {
            margin-top: 10px;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            justify-content: center;
        }

        .platform-link {
            font-family: 'Poppins', sans-serif;
            font-weight: 400;
            padding: 8px 15px;
            border-radius: 5px;
            text-decoration: none;
            color: #fff;
            font-size: 1.1em;
            transition: all 0.3s ease;
            background: rgba(255, 255, 255, 0.1);
        }

        .platform-link:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }

        .platform-link.netflix { background: rgba(229, 9, 20, 0.6); }
        .platform-link.amazon { background: rgba(0, 168, 225, 0.6); }
        .platform-link.disney { background: rgba(113, 107, 248, 0.6); }
        .platform-link.hulu { background: rgba(28, 231, 131, 0.6); }
        .platform-link.hbomax { background: rgba(188, 0, 255, 0.6); }
        .platform-link.paramount { background: rgba(0, 99, 233, 0.6); }
        .platform-link.youtube { background: rgba(255, 0, 0, 0.6); }
        .platform-link.apple { background: rgba(0, 0, 0, 0.6); }
        .platform-link.crunchyroll { background: rgba(247, 117, 0, 0.6); }
        .platform-link.peacock { background: rgba(0, 136, 204, 0.6); }

        .no-platforms {
            color: #7f8c8d;
            font-size: 0.9em;
            font-style: italic;
        }

        #results h2 {
            font-family: 'Outfit', sans-serif;
            font-weight: 600;
            font-size: 2em;
            color: #00fff5;
            margin-bottom: 30px;
            text-align: center;
            text-shadow: 0 0 15px rgba(0, 255, 245, 0.3);
        }

        .popcorn-animation {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            overflow: hidden;
        }

        .popcorn-container {
            position: absolute;
            width: 100%;
            height: 100%;
        }

        .popcorn {
            position: absolute;
            width: 30px;
            height: 30px;
            background: radial-gradient(circle at 30% 30%, #ffffff 5%, #FFF9E6 30%, #F5DEB3 60%, #DEB887 90%);
            border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
            box-shadow: 
                inset -2px -2px 4px rgba(0,0,0,0.3),
                2px 2px 4px rgba(255,255,255,0.3);
            transform: rotate(var(--rotation));
            animation: popUpNew 2s ease-out forwards;
            opacity: 0;
        }

        .popcorn::before {
            content: '';
            position: absolute;
            top: 20%;
            left: 20%;
            width: 20%;
            height: 20%;
            background: rgba(255,255,255,0.8);
            border-radius: 50%;
        }

        .popcorn::after {
            content: '';
            position: absolute;
            bottom: 15%;
            right: 15%;
            width: 15%;
            height: 15%;
            background: rgba(222,184,135,0.5);
            border-radius: 50%;
        }

        @keyframes popUpNew {
            0% {
                transform: 
                    translateY(100vh) 
                    translateX(var(--translateX))
                    rotate(var(--rotation))
                    scale(0);
                opacity: 0;
            }
            30% {
                opacity: 1;
            }
            100% {
                transform: 
                    translateY(var(--finalY)) 
                    translateX(var(--translateX))
                    rotate(var(--finalRotation))
                    scale(1);
                opacity: var(--finalOpacity);
            }
        }

        .fade-out {
            animation: fadeOut 0.5s ease-out forwards;
        }

        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }

        .movie-stats {
            margin-top: 10px;
            padding-top: 10px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            font-size: 0.9em;
            color: rgba(255, 255, 255, 0.7);
        }

        .rating-bar {
            width: 100%;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            margin: 5px 0;
        }

        .rating-fill {
            height: 100%;
            background: #00fff5;
            border-radius: 2px;
            transition: width 1s ease;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 5px;
            margin-top: 8px;
            font-size: 0.85em;
        }

        .stat-item {
            text-align: center;
            padding: 3px;
        }

        .stat-label {
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Bored at Night</h1>
            <p class="subtitle">Take a quick quiz to find your perfect movie for tonight!</p>
        </div>

        <div class="quiz-container">
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>

            <div class="question active" data-question="1">
                <h2>How old are you?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('age', 'kid')">Under 13</div>
                    <div class="option" onclick="nextQuestion('age', 'teen')">13-17</div>
                    <div class="option" onclick="nextQuestion('age', 'adult')">18+</div>
                </div>
            </div>

            <div class="question" data-question="2">
                <h2>What's your current mood?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('mood', 'happy')">Feeling happy and upbeat</div>
                    <div class="option" onclick="nextQuestion('mood', 'thoughtful')">Thoughtful and introspective</div>
                    <div class="option" onclick="nextQuestion('mood', 'excited')">Looking for excitement</div>
                    <div class="option" onclick="nextQuestion('mood', 'relaxed')">Just want to relax</div>
                </div>
            </div>

            <div class="question" data-question="3">
                <h2>How much time do you have?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('duration', 'short')">Less than 2 hours</div>
                    <div class="option" onclick="nextQuestion('duration', 'medium')">2-3 hours</div>
                    <div class="option" onclick="nextQuestion('duration', 'long')">I have all night</div>
                </div>
            </div>

            <div class="question" data-question="4">
                <h2>Who are you watching with?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('company', 'alone')">Just me</div>
                    <div class="option" onclick="nextQuestion('company', 'family')">Family</div>
                    <div class="option" onclick="nextQuestion('company', 'friends')">Friends</div>
                    <div class="option" onclick="nextQuestion('company', 'date')">Date night</div>
                </div>
            </div>

            <div class="question" data-question="5">
                <h2>What genres do you enjoy?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('genre', 'action')">Action/Adventure</div>
                    <div class="option" onclick="nextQuestion('genre', 'comedy')">Comedy</div>
                    <div class="option" onclick="nextQuestion('genre', 'drama')">Drama/Romance</div>
                    <div class="option" onclick="nextQuestion('genre', 'scifi')">Sci-Fi/Fantasy</div>
                    <div class="option" onclick="nextQuestion('genre', 'animation')">Animation</div>
                </div>
            </div>

            <div class="question" data-question="6">
                <h2>Prefer something new or classic?</h2>
                <div class="options">
                    <div class="option" onclick="nextQuestion('era', 'new')">Recent releases</div>
                    <div class="option" onclick="nextQuestion('era', 'recent')">Last few years</div>
                    <div class="option" onclick="nextQuestion('era', 'classic')">Classic movies</div>
                    <div class="option" onclick="nextQuestion('era', 'any')">Don't mind</div>
                </div>
            </div>

            <div class="loading" id="loading">
                <p class="loading-text">Our AI is finding your perfect movies...</p>
                <div class="dots">
                    <div class="dot"></div>
                    <div class="dot"></div>
                    <div class="dot"></div>
                </div>
            </div>

            <div id="results">
                <h2>Based on your preferences, you might enjoy:</h2>
                <div class="movie-list" id="movieList"></div>
            </div>
        </div>

        <div class="popcorn-animation" id="popcornAnimation">
            <div class="popcorn-container" id="popcornContainer"></div>
        </div>
    </div>

    <script>
        let currentQuestion = 1;
        const totalQuestions = 5;
        const userPreferences = {};

        const movieDatabase = {
            action: {
                new: [
                    { 
                        title: "Mission: Impossible - Dead Reckoning", 
                        genre: "Action/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/NNxYkU70HPurnNCSiCjYAmacwm.jpg",
                        platforms: {
                            paramount: "https://www.paramountplus.com/movies/mission-impossible-dead-reckoning/",
                            amazon: "https://www.amazon.com/Mission-Impossible-Dead-Reckoning-Part/dp/B0C8MJM7KT"
                        },
                        rating: 83,
                        stats: {
                            runtime: "155 min",
                            year: "2021",
                            director: "Christopher McQuarrie",
                            boxOffice: "$402M"
                        }
                    },
                    { 
                        title: "John Wick: Chapter 4",
                        genre: "Action/Thriller",
                        poster: "https://image.tmdb.org/t/p/w500/vZloFAK7NmvMGKE7VkF5UHaz0I.jpg",
                        platforms: {
                            amazon: "https://www.amazon.com/John-Wick-Chapter-Keanu-Reeves/dp/B0BYZ3QYP5",
                            youtube: "https://www.youtube.com/watch?v=qEVUtrk8_B4"
                        },
                        rating: 83,
                        stats: {
                            runtime: "159 min",
                            year: "2023",
                            director: "Chad Stahelski",
                            boxOffice: "$326M"
                        }
                    },
                    { 
                        title: "The Batman", 
                        genre: "Action/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/74xTEgt7R36Fpooo50r9T25onhq.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/the-batman/",
                            amazon: "https://www.amazon.com/Batman-Robert-Pattinson/dp/B09VGKXRNX"
                        },
                        rating: 83,
                        stats: {
                            runtime: "176 min",
                            year: "2022",
                            director: "Matt Reeves",
                            boxOffice: "$767M"
                        }
                    },
                    { 
                        title: "Top Gun: Maverick", 
                        genre: "Action/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/62HCnUTziyWcpDaBO2i1DX17ljH.jpg",
                        platforms: {
                            paramount: "https://www.paramountplus.com/movies/top-gun-maverick/",
                            amazon: "https://www.amazon.com/Top-Gun-Maverick-Tom-Cruise/dp/B0B3CKDLQS"
                        },
                        rating: 83,
                        stats: {
                            runtime: "130 min",
                            year: "2022",
                            director: "Joseph Kosinski",
                            boxOffice: "$791M"
                        }
                    },
                    {
                        title: "The Gray Man",
                        genre: "Action/Thriller",
                        poster: "https://image.tmdb.org/t/p/w500/8cXbitsS6dWQ5gfMTZdorpAAzEH.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/81160697"
                        },
                        rating: 74,
                        stats: {
                            runtime: "122 min",
                            year: "2022",
                            director: "Anthony and Joe Russo",
                            boxOffice: "$200M"
                        }
                    }
                ],
                classic: [
                    { 
                        title: "Die Hard", 
                        genre: "Action/Thriller",
                        poster: "https://image.tmdb.org/t/p/w500/yFihWxQcmqcaBR31QM6Y8gT6aYV.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/die-hard/",
                            amazon: "https://www.amazon.com/Die-Hard-Bruce-Willis/dp/B003QXBW3Y"
                        },
                        rating: 94,
                        stats: {
                            runtime: "131 min",
                            year: "1988",
                            director: "John McTiernan",
                            boxOffice: "$87.8M"
                        }
                    },
                    { 
                        title: "Indiana Jones Series", 
                        genre: "Action/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/ceG9VzoRAVGwivFU403Wc3AHRys.jpg",
                        platforms: {
                            paramount: "https://www.paramountplus.com/movies/indiana-jones/",
                            disney: "https://www.disneyplus.com/franchise/indiana-jones"
                        },
                        rating: 95,
                        stats: {
                            runtime: "125 min",
                            year: "1981",
                            director: "Steven Spielberg",
                            boxOffice: "$297M"
                        }
                    },
                    { 
                        title: "The Matrix", 
                        genre: "Action/Sci-Fi",
                        poster: "https://image.tmdb.org/t/p/w500/f89U3ADr1oiB1s9GkdPOEpXUk5H.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/the-matrix/",
                            amazon: "https://www.amazon.com/Matrix-Keanu-Reeves/dp/B000HAB4KI"
                        },
                        rating: 88,
                        stats: {
                            runtime: "136 min",
                            year: "1999",
                            director: "The Wachowskis",
                            boxOffice: "$171M"
                        }
                    },
                    {
                        title: "Terminator 2: Judgment Day",
                        genre: "Action/Sci-Fi",
                        poster: "https://image.tmdb.org/t/p/w500/5M0j0B18abtBI5gi2RhfjjurTqb.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/60028202",
                            amazon: "https://www.amazon.com/Terminator-2-Judgment-Day-Schwarzenegger/dp/B00153ZC8Q"
                        },
                        rating: 93,
                        stats: {
                            runtime: "137 min",
                            year: "1991",
                            director: "James Cameron",
                            boxOffice: "$520M"
                        }
                    },
                    {
                        title: "Mad Max: Fury Road",
                        genre: "Action/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/8tZYtuWezp8JbcsvHYO0O46tFbo.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/mad-max-fury-road/",
                            amazon: "https://www.amazon.com/Mad-Max-Fury-Tom-Hardy/dp/B00XJXG8P0"
                        },
                        rating: 97,
                        stats: {
                            runtime: "120 min",
                            year: "2015",
                            director: "George Miller",
                            boxOffice: "$378M"
                        }
                    }
                ]
            },
            comedy: {
                new: [
                    { 
                        title: "Barbie",
                        genre: "Comedy/Fantasy",
                        poster: "https://image.tmdb.org/t/p/w500/iuFNMS8U5cb6xfzi51Dbkovj7vM.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/barbie/",
                            amazon: "https://www.amazon.com/Barbie-Margot-Robbie/dp/B0CBQL42K3"
                        },
                        rating: 88,
                        stats: {
                            runtime: "114 min",
                            year: "2023",
                            director: "Greta Gerwig",
                            boxOffice: "$144M"
                        }
                    },
                    {
                        title: "Free Guy",
                        genre: "Comedy/Action",
                        poster: "https://image.tmdb.org/t/p/w500/xmbU4JTUm8rsdtn7Y3Fcm30GpeT.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/free-guy/",
                            amazon: "https://www.amazon.com/Free-Guy-Ryan-Reynolds/dp/B09CNKCGT9"
                        },
                        rating: 80,
                        stats: {
                            runtime: "115 min",
                            year: "2021",
                            director: "Shawn Levy",
                            boxOffice: "$331M"
                        }
                    },
                    {
                        title: "The Menu",
                        genre: "Dark Comedy/Thriller",
                        poster: "https://image.tmdb.org/t/p/w500/fPtUgMcLIboqlTlPrq0bQpKK8eq.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/the-menu/",
                            amazon: "https://www.amazon.com/Menu-Ralph-Fiennes/dp/B0BN5C9Z3L"
                        },
                        rating: 88,
                        stats: {
                            runtime: "107 min",
                            year: "2022",
                            director: "Mark Mylod",
                            boxOffice: "$14.5M"
                        }
                    }
                ],
                classic: [
                    {
                        title: "The Hangover",
                        genre: "Comedy",
                        poster: "https://image.tmdb.org/t/p/w500/uluhlXubGu1VxU63X9VHCLWDAYP.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/70113005",
                            amazon: "https://www.amazon.com/Hangover-Bradley-Cooper/dp/B002PCHGMG"
                        },
                        rating: 79,
                        stats: {
                            runtime: "100 min",
                            year: "2009",
                            director: "Todd Phillips",
                            boxOffice: "$46.6M"
                        }
                    },
                    {
                        title: "Groundhog Day",
                        genre: "Comedy/Fantasy",
                        poster: "https://image.tmdb.org/t/p/w500/gCgt1WARPZaXnq523ySQEUKinCs.jpg",
                        platforms: {
                            amazon: "https://www.amazon.com/Groundhog-Day-Bill-Murray/dp/B000CNESQU",
                            netflix: "https://www.netflix.com/title/563104"
                        },
                        rating: 96,
                        stats: {
                            runtime: "91 min",
                            year: "1993",
                            director: "Harold Ramis",
                            boxOffice: "$57.3M"
                        }
                    },
                    {
                        title: "Back to the Future",
                        genre: "Comedy/Sci-Fi",
                        poster: "https://image.tmdb.org/t/p/w500/fNOH9f1aA7XRTzl1sAOx9iF553Q.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/60010110",
                            amazon: "https://www.amazon.com/Back-Future-Michael-J-Fox/dp/B0011E5JIU"
                        },
                        rating: 96,
                        stats: {
                            runtime: "116 min",
                            year: "1985",
                            director: "Robert Zemeckis",
                            boxOffice: "$381M"
                        }
                    }
                ]
            },
            drama: {
                new: [
                    {
                        title: "Oppenheimer",
                        genre: "Drama/Biography",
                        poster: "https://image.tmdb.org/t/p/w500/8Gxv8gSFCU0XGDykEGv7zR1n2ua.jpg",
                        platforms: {
                            peacock: "https://www.peacocktv.com/watch/asset/movies/oppenheimer/",
                            amazon: "https://www.amazon.com/Oppenheimer-Cillian-Murphy/dp/B0CGQX3H8P"
                        },
                        rating: 93,
                        stats: {
                            runtime: "180 min",
                            year: "2023",
                            director: "Christopher Nolan",
                            boxOffice: "$179M"
                        }
                    },
                    {
                        title: "Everything Everywhere All at Once",
                        genre: "Drama/Sci-Fi",
                        poster: "https://image.tmdb.org/t/p/w500/w3LxiVYdWWRvEVdn5RYq6jIqkb1.jpg",
                        platforms: {
                            paramount: "https://www.paramountplus.com/movies/everything-everywhere-all-at-once/",
                            amazon: "https://www.amazon.com/Everything-Everywhere-All-Michelle-Yeoh/dp/B09WN4HSWX"
                        },
                        rating: 95,
                        stats: {
                            runtime: "139 min",
                            year: "2022",
                            director: "Daniel Kwan and Daniel Scheinert",
                            boxOffice: "$100M"
                        }
                    },
                    {
                        title: "The Whale",
                        genre: "Drama",
                        poster: "https://image.tmdb.org/t/p/w500/jQ0gylJMxWSL490sy0RrPj1Lj7e.jpg",
                        platforms: {
                            hulu: "https://www.hulu.com/movie/the-whale",
                            amazon: "https://www.amazon.com/Whale-Brendan-Fraser/dp/B0BQ6YVKBQ"
                        },
                        rating: 65,
                        stats: {
                            runtime: "117 min",
                            year: "2022",
                            director: "Darren Aronofsky",
                            boxOffice: "$17.5M"
                        }
                    }
                ],
                classic: [
                    {
                        title: "The Shawshank Redemption",
                        genre: "Drama",
                        poster: "https://image.tmdb.org/t/p/w500/q6y0Go1tsGEsmtFryDOJo3dEmqu.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/70005379",
                            amazon: "https://www.amazon.com/Shawshank-Redemption-Tim-Robbins/dp/B001EBBOF0"
                        },
                        rating: 98,
                        stats: {
                            runtime: "142 min",
                            year: "1994",
                            director: "Frank Darabont",
                            boxOffice: "$28.3M"
                        }
                    },
                    {
                        title: "Forrest Gump",
                        genre: "Drama/Romance",
                        poster: "https://image.tmdb.org/t/p/w500/arw2vcBveWOVZr6pxd9XTd1TdQa.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/60000724",
                            paramount: "https://www.paramountplus.com/movies/forrest-gump/"
                        },
                        rating: 71,
                        stats: {
                            runtime: "142 min",
                            year: "1994",
                            director: "Robert Zemeckis",
                            boxOffice: "$329.7M"
                        }
                    },
                    {
                        title: "Good Will Hunting",
                        genre: "Drama",
                        poster: "https://image.tmdb.org/t/p/w500/bABCBKYBK7A5G1x0FzoeoNfuj2.jpg",
                        platforms: {
                            hulu: "https://www.hulu.com/movie/good-will-hunting",
                            amazon: "https://www.amazon.com/Good-Will-Hunting-Robin-Williams/dp/B006RXPNJS"
                        },
                        rating: 98,
                        stats: {
                            runtime: "126 min",
                            year: "1997",
                            director: "Gus Van Sant",
                            boxOffice: "$173.2M"
                        }
                    }
                ]
            },
            scifi: {
                new: [
                    {
                        title: "Dune",
                        genre: "Sci-Fi/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/d5NXSklXo0qyIYkgV94XAgMIckC.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/dune/",
                            amazon: "https://www.amazon.com/Dune-Timothée-Chalamet/dp/B09JZSWW3P",
                            youtube: "https://www.youtube.com/watch?v=8g18jFHCLXk"
                        },
                        rating: 83,
                        stats: {
                            runtime: "155 min",
                            year: "2021",
                            director: "Denis Villeneuve",
                            boxOffice: "$402M"
                        }
                    },
                    {
                        title: "Blade Runner 2049",
                        genre: "Sci-Fi/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/gajva2L0rPYkEWjzgFlBXCAVBE5.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/80185346",
                            amazon: "https://www.amazon.com/Blade-Runner-2049-Harrison-Ford/dp/B0777VH1K1"
                        },
                        rating: 88,
                        stats: {
                            runtime: "163 min",
                            year: "2017",
                            director: "Denis Villeneuve",
                            boxOffice: "$100M"
                        }
                    },
                    {
                        title: "Inception",
                        genre: "Sci-Fi/Action",
                        poster: "https://image.tmdb.org/t/p/w500/8IB2e4r4oVhHnANbnm7O3Tj6tF8.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/inception/",
                            amazon: "https://www.amazon.com/Inception-Leonardo-DiCaprio/dp/B0047WJ11G"
                        },
                        rating: 87,
                        stats: {
                            runtime: "148 min",
                            year: "2010",
                            director: "Christopher Nolan",
                            boxOffice: "$292.5M"
                        }
                    }
                ],
                classic: [
                    {
                        title: "Star Wars Original Trilogy",
                        genre: "Sci-Fi/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/6FfCtAuVAW8XJjZ7eWeLibRLWTw.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/franchise/star-wars",
                            amazon: "https://www.amazon.com/Star-Wars-New-Hope-Theatrical/dp/B00VF06OBS"
                        },
                        rating: 93,
                        stats: {
                            runtime: "121 min",
                            year: "1977",
                            director: "George Lucas",
                            boxOffice: "$775.4M"
                        }
                    },
                    {
                        title: "Alien",
                        genre: "Sci-Fi/Horror",
                        poster: "https://image.tmdb.org/t/p/w500/vfrQk5IPloGg1v9Rzbh2Eg3VGyM.jpg",
                        platforms: {
                            hulu: "https://www.hulu.com/movie/alien",
                            disney: "https://www.disneyplus.com/movies/alien/"
                        },
                        rating: 98,
                        stats: {
                            runtime: "117 min",
                            year: "1979",
                            director: "Ridley Scott",
                            boxOffice: "$128.7M"
                        }
                    },
                    {
                        title: "2001: A Space Odyssey",
                        genre: "Sci-Fi/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/ve72VxNqjGM69Uky4WTo2bK6rfq.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/2001-a-space-odyssey/",
                            amazon: "https://www.amazon.com/2001-Space-Odyssey-Keir-Dullea/dp/B001OF3AQI"
                        },
                        rating: 92,
                        stats: {
                            runtime: "142 min",
                            year: "1968",
                            director: "Stanley Kubrick",
                            boxOffice: "$134.9M"
                        }
                    }
                ]
            },
            animation: {
                new: [
                    {
                        title: "Spider-Man: Across the Spider-Verse",
                        genre: "Animation/Action",
                        poster: "https://image.tmdb.org/t/p/w500/8Vt6mWEReuy4Of61Lnj5Xj704m8.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/81670614",
                            amazon: "https://www.amazon.com/Spider-Man-Across-Spider-Verse-Shameik-Moore/dp/B0C6GJBXG7"
                        },
                        rating: 96,
                        stats: {
                            runtime: "120 min",
                            year: "2023",
                            director: "Joaquim Dos Santos and Kemp Powers",
                            boxOffice: "$262M"
                        }
                    },
                    {
                        title: "The Super Mario Bros. Movie",
                        genre: "Animation/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/qNBAXBIQlnOThrVvA6mA2B5ggV6.jpg",
                        platforms: {
                            peacock: "https://www.peacocktv.com/watch/asset/movies/the-super-mario-bros-movie/",
                            amazon: "https://www.amazon.com/Super-Mario-Bros-Movie-Chris-Pratt/dp/B0BZ418BWX"
                        },
                        rating: 87,
                        stats: {
                            runtime: "92 min",
                            year: "2023",
                            director: "Aaron Horvath and Michael Jelenic",
                            boxOffice: "$456M"
                        }
                    },
                    {
                        title: "Elemental",
                        genre: "Animation/Romance",
                        poster: "https://image.tmdb.org/t/p/w500/4Y1WNkd88JXmGfhtWR7dmDAo1T2.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/elemental/",
                            amazon: "https://www.amazon.com/Elemental-Leah-Lewis/dp/B0C7RJVVZ9"
                        },
                        rating: 76,
                        stats: {
                            runtime: "102 min",
                            year: "2023",
                            director: "Peter Sohn",
                            boxOffice: "$140M"
                        }
                    },
                    {
                        title: "Suzume",
                        genre: "Animation/Fantasy",
                        poster: "https://image.tmdb.org/t/p/w500/vIeu8WysZrTSFb2uhPViKjX9EcC.jpg",
                        platforms: {
                            crunchyroll: "https://www.crunchyroll.com/movies/suzume/",
                            amazon: "https://www.amazon.com/Suzume-Nanoka-Hara/dp/B0BXTKLVZ3"
                        },
                        rating: 95,
                        stats: {
                            runtime: "122 min",
                            year: "2023",
                            director: "Hayao Miyazaki",
                            boxOffice: "$100M"
                        }
                    },
                    {
                        title: "Puss in Boots: The Last Wish",
                        genre: "Animation/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/kuf6dutpsT0vSVehic3EZIqkOBt.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/81227328",
                            peacock: "https://www.peacocktv.com/watch/asset/movies/puss-in-boots-the-last-wish/"
                        },
                        rating: 95,
                        stats: {
                            runtime: "92 min",
                            year: "2022",
                            director: "Joel Crawford",
                            boxOffice: "$165M"
                        }
                    }
                ],
                classic: [
                    {
                        title: "Spirited Away",
                        genre: "Animation/Fantasy",
                        poster: "https://image.tmdb.org/t/p/w500/39wmItIWsg5sZMyRUHLkWBcuVCM.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/spirited-away/",
                            netflix: "https://www.netflix.com/title/60023642"
                        },
                        rating: 97,
                        stats: {
                            runtime: "125 min",
                            year: "2001",
                            director: "Hayao Miyazaki",
                            boxOffice: "$165.5M"
                        }
                    },
                    {
                        title: "The Lion King",
                        genre: "Animation/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/sKCr78MXSLixwmZ8DyJLrpMsd15.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/the-lion-king/",
                            amazon: "https://www.amazon.com/Lion-King-Matthew-Broderick/dp/B0094CTQ2M"
                        },
                        rating: 93,
                        stats: {
                            runtime: "88 min",
                            year: "1994",
                            director: "Roger Allers and Rob Minkoff",
                            boxOffice: "$704.8M"
                        }
                    },
                    {
                        title: "Toy Story",
                        genre: "Animation/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/uXDfjJbdP4ijW5hWSBrPrlKpxab.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/toy-story/",
                            amazon: "https://www.amazon.com/Toy-Story-Tom-Hanks/dp/B0049J0DC8"
                        },
                        rating: 98,
                        stats: {
                            runtime: "81 min",
                            year: "1995",
                            director: "John Lasseter",
                            boxOffice: "$352.4M"
                        }
                    },
                    {
                        title: "WALL·E",
                        genre: "Kids/Animation",
                        poster: "https://lumiere-a.akamaihd.net/v1/images/p_walle_19753_69f7ff00.jpeg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/wall-e/",
                            amazon: "https://www.amazon.com/WALL-Ben-Burtt/dp/B001EIOOV8"
                        },
                        rating: 95,
                        stats: {
                            runtime: "98 min",
                            year: "2008",
                            director: "Andrew Stanton",
                            boxOffice: "$215.4M"
                        }
                    },
                    {
                        title: "Princess Mononoke",
                        genre: "Animation/Fantasy",
                        poster: "https://m.media-amazon.com/images/M/MV5BNGIzY2IzODQtNThmMi00ZDE4LWI5YzAtNzNlZTM1ZjYyYjUyXkEyXkFqcGdeQXVyODEzNjM5OTQ@._V1_.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/princess-mononoke/",
                            netflix: "https://www.netflix.com/title/28630857"
                        },
                        rating: 83,
                        stats: {
                            runtime: "134 min",
                            year: "1997",
                            director: "Hayao Miyazaki",
                            boxOffice: "$161.5M"
                        }
                    },
                    {
                        title: "How to Train Your Dragon",
                        genre: "Animation/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/ygGmAO60t8GyqUo9xYeYxSZAR3b.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/70120977",
                            amazon: "https://www.amazon.com/How-Train-Your-Dragon-Baruchel/dp/B003AQBX6U"
                        },
                        rating: 83,
                        stats: {
                            runtime: "98 min",
                            year: "2010",
                            director: "Dean DeBlois",
                            boxOffice: "$494.5M"
                        }
                    },
                    {
                        title: "Grave of the Fireflies",
                        genre: "Animation/Drama",
                        poster: "https://image.tmdb.org/t/p/w500/k9tv1rXZbOhH7eiCk378x61kNQ1.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/grave-of-the-fireflies/",
                            crunchyroll: "https://www.crunchyroll.com/watch/GY8VX0PQY/grave-of-the-fireflies"
                        },
                        rating: 97,
                        stats: {
                            runtime: "89 min",
                            year: "1988",
                            director: "Isao Takahata",
                            boxOffice: "$516K"
                        }
                    }
                ]
            },
            kids: {
                new: [
                    {
                        title: "Moana",
                        genre: "Kids/Animation",
                        poster: "https://lumiere-a.akamaihd.net/v1/images/p_moana_20530_214883e3.jpeg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/moana/",
                            amazon: "https://www.amazon.com/Moana-Auli-i-Cravalho/dp/B01MSDYRNE"
                        },
                        rating: 95,
                        stats: {
                            runtime: "107 min",
                            year: "2016",
                            director: "Ron Clements and John Musker",
                            boxOffice: "$144.4M"
                        }
                    },
                    {
                        title: "Encanto",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/4j0PNHkMr5ax3IA8tjtxcmPU3QT.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/encanto/",
                            amazon: "https://www.amazon.com/Encanto-Stephanie-Beatriz/dp/B09MXRDP31"
                        },
                        rating: 91,
                        stats: {
                            runtime: "102 min",
                            year: "2021",
                            director: "Jared Bush and Byron Howard",
                            boxOffice: "$145.6M"
                        }
                    },
                    {
                        title: "The Bad Guys",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/7qop80YfuO0BwJa1uXk1DXUUEwv.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/81332733",
                            peacock: "https://www.peacocktv.com/watch/asset/movies/the-bad-guys/"
                        },
                        rating: 88,
                        stats: {
                            runtime: "100 min",
                            year: "2022",
                            director: "Pierre Perifel",
                            boxOffice: "$140M"
                        }
                    },
                    {
                        title: "Sonic the Hedgehog 2",
                        genre: "Kids/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/6DrHO1jr3qVrViUO6s6kFiAGM7.jpg",
                        platforms: {
                            paramount: "https://www.paramountplus.com/movies/sonic-the-hedgehog-2/",
                            amazon: "https://www.amazon.com/Sonic-Hedgehog-2-James-Marsden/dp/B09VGKZ7VH"
                        },
                        rating: 88,
                        stats: {
                            runtime: "92 min",
                            year: "2022",
                            director: "Jeff Fowler",
                            boxOffice: "$148.7M"
                        }
                    },
                    {
                        title: "Turning Red",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/qsdjk9oAKSQMWs0Vt5Pyfh6O4GZ.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/turning-red/",
                            amazon: "https://www.amazon.com/Turning-Red-Rosalie-Chiang/dp/B09NRVF1XR"
                        },
                        rating: 95,
                        stats: {
                            runtime: "100 min",
                            year: "2022",
                            director: "Domee Shi",
                            boxOffice: "$140M"
                        }
                    },
                    {
                        title: "Luca",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/jTswp6KyDYKtvC52GbHagrZbGvD.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/luca/",
                            amazon: "https://www.amazon.com/Luca-Jacob-Tremblay/dp/B096WL13ZR"
                        },
                        rating: 91,
                        stats: {
                            runtime: "102 min",
                            year: "2021",
                            director: "Enrico Casarosa",
                            boxOffice: "$140M"
                        }
                    },
                    {
                        title: "The Sea Beast",
                        genre: "Kids/Adventure",
                        poster: "https://image.tmdb.org/t/p/w500/9Zfv4Ap1e8eKOYnZPtYaWhLkk0d.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/81018682"
                        },
                        rating: 94,
                        stats: {
                            runtime: "105 min",
                            year: "2022",
                            director: "Chris Williams",
                            boxOffice: "$140M"
                        }
                    },
                    {
                        title: "DC League of Super-Pets",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/r7XifzvtezNt31ypvsmb6Oqxw49.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/dc-league-of-super-pets/",
                            amazon: "https://www.amazon.com/DC-League-Super-Pets-Dwayne-Johnson/dp/B0B6WRWPH5"
                        },
                        rating: 77,
                        stats: {
                            runtime: "101 min",
                            year: "2022",
                            director: "Jared Stern",
                            boxOffice: "$140M"
                        }
                    }
                ],
                classic: [
                    {
                        title: "The Incredibles",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/2LqaLgk4Z226KkgPJuiOQ58wvrm.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/the-incredibles/",
                            amazon: "https://www.amazon.com/Incredibles-Craig-T-Nelson/dp/B0094KT5H2"
                        },
                        rating: 97,
                        stats: {
                            runtime: "115 min",
                            year: "2004",
                            director: "Brad Bird",
                            boxOffice: "$261.4M"
                        }
                    },
                    {
                        title: "Finding Nemo",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/eHuGQ10FUzK1mdOY69wF5pGgEf5.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/finding-nemo/",
                            amazon: "https://www.amazon.com/Finding-Nemo-Albert-Brooks/dp/B00HQDAFW0"
                        },
                        rating: 99,
                        stats: {
                            runtime: "100 min",
                            year: "2003",
                            director: "Andrew Stanton",
                            boxOffice: "$380.8M"
                        }
                    },
                    {
                        title: "Shrek",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/dyhaB19AICF7TO7CK2aD6KfymnQ.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/60020686",
                            amazon: "https://www.amazon.com/Shrek-Mike-Myers/dp/B00005JKQW"
                        },
                        rating: 88,
                        stats: {
                            runtime: "90 min",
                            year: "2001",
                            director: "Andrew Adamson and Vicky Jenson",
                            boxOffice: "$267.7M"
                        }
                    },
                    {
                        title: "Kung Fu Panda",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/wWt4JYXTg5Wr3xBW2phBrMKgp3x.jpg",
                        platforms: {
                            netflix: "https://www.netflix.com/title/70075480",
                            amazon: "https://www.amazon.com/Kung-Fu-Panda-Jack-Black/dp/B001NXDFHM"
                        },
                        rating: 87,
                        stats: {
                            runtime: "90 min",
                            year: "2008",
                            director: "Mark Osborne and John Stevenson",
                            boxOffice: "$215.4M"
                        }
                    },
                    {
                        title: "Ratatouille",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/t3vaWRPSf6WjDSamIkKDs1iQWna.jpg",
                        platforms: {
                            disney: "https://www.disneyplus.com/movies/ratatouille/",
                            amazon: "https://www.amazon.com/Ratatouille-Patton-Oswalt/dp/B0094KTFA6"
                        },
                        rating: 96,
                        stats: {
                            runtime: "115 min",
                            year: "2007",
                            director: "Brad Bird",
                            boxOffice: "$206.4M"
                        }
                    },
                    {
                        title: "The Iron Giant",
                        genre: "Kids/Animation",
                        poster: "https://image.tmdb.org/t/p/w500/bwVhmPpydv8P7mWfrmL3XVw0MV5.jpg",
                        platforms: {
                            hbomax: "https://www.max.com/movies/the-iron-giant/",
                            amazon: "https://www.amazon.com/Iron-Giant-Eli-Marienthal/dp/B001EBWIOC"
                        },
                        rating: 96,
                        stats: {
                            runtime: "89 min",
                            year: "1999",
                            director: "Brad Bird",
                            boxOffice: "$17.5M"
                        }
                    }
                ]
            }
        };

        function nextQuestion(category, value) {
            userPreferences[category] = value;
            
            if (currentQuestion < totalQuestions) {
                const currentQuestionElement = document.querySelector(`[data-question="${currentQuestion}"]`);
                const nextQuestionElement = document.querySelector(`[data-question="${currentQuestion + 1}"]`);
                
                // Add exit animation to current question
                currentQuestionElement.classList.add('exit');
                
                setTimeout(() => {
                    currentQuestionElement.classList.remove('active', 'exit');
                    currentQuestion++;
                    nextQuestionElement.classList.add('active');
                    updateProgress();
                    
                    // Reset option animations
                    const options = nextQuestionElement.querySelectorAll('.option');
                    options.forEach((option, index) => {
                        option.style.animationDelay = `${0.1 * (index + 1)}s`;
                        option.style.animation = 'none';
                        option.offsetHeight; // Trigger reflow
                        option.style.animation = null;
                    });
                }, 500);
            } else {
                showLoading();
            }
        }

        function updateProgress() {
            // Calculate progress including the current question
            const progress = (currentQuestion / totalQuestions) * 100;
            document.getElementById('progress').style.width = `${progress}%`;
        }

        function showLoading() {
            document.querySelector('.question.active').style.display = 'none';
            document.getElementById('loading').style.display = 'block';
            setTimeout(() => {
                document.getElementById('loading').style.display = 'none';
                showPopcornAnimation();
            }, 2000);
        }

        function showPopcornAnimation() {
            const popcornAnimation = document.getElementById('popcornAnimation');
            popcornAnimation.style.display = 'flex';
            createPopcorn();
            
            setTimeout(() => {
                popcornAnimation.classList.add('fade-out');
                setTimeout(() => {
                    popcornAnimation.style.display = 'none';
                    popcornAnimation.classList.remove('fade-out');
                    showResults();
                }, 500);
            }, 2000);
        }

        function createPopcorn() {
            const container = document.getElementById('popcornContainer');
            container.innerHTML = ''; // Clear existing popcorn

            const numberOfPopcorns = 100; // Increased number for fuller screen
            
            for (let i = 0; i < numberOfPopcorns; i++) {
                const popcorn = document.createElement('div');
                popcorn.className = 'popcorn';
                
                // Random positions and animations
                const translateX = Math.random() * 100 - 50 + 'vw';
                const finalY = Math.random() * 100 + 'vh';
                const rotation = Math.random() * 360 + 'deg';
                const finalRotation = Math.random() * 720 - 360 + 'deg';
                const delay = Math.random() * 1.5 + 's';
                const finalOpacity = Math.random() * 0.5 + 0.5;
                
                popcorn.style.cssText = `
                    left: ${Math.random() * 100}vw;
                    --translateX: ${translateX};
                    --finalY: -${finalY};
                    --rotation: ${rotation};
                    --finalRotation: ${finalRotation};
                    --finalOpacity: ${finalOpacity};
                    animation-delay: ${delay};
                `;
                
                container.appendChild(popcorn);
            }
        }

        function showResults() {
            document.getElementById('loading').style.display = 'none';
            document.getElementById('results').style.display = 'block';

            const movieList = document.getElementById('movieList');
            movieList.innerHTML = '';

            const movies = getRecommendedMovies();

            movies.forEach(movie => {
                const movieCard = document.createElement('div');
                movieCard.className = 'movie-card';
                
                // Create platform links HTML
                let platformLinks = '';
                if (movie.platforms) {
                    platformLinks = Object.entries(movie.platforms).map(([platform, url]) => `
                        <a href="${url}" target="_blank" class="platform-link ${platform}">
                            ${getPlatformIcon(platform)} ${platform}
                        </a>
                    `).join('');
                } else {
                    // Suggest common platforms based on movie genre and era
                    const suggestions = getSuggestedPlatforms(movie.genre, movie.title);
                    platformLinks = `<span class="no-platforms">Available on: ${suggestions}</span>`;
                }

                movieCard.innerHTML = `
                    <img src="${movie.poster}" alt="${movie.title}" class="movie-poster">
                    <div class="movie-title">${movie.title}</div>
                    <div class="movie-genre">${movie.genre}</div>
                    ${movie.rating ? `
                        <div class="movie-stats">
                            <div style="display: flex; justify-content: space-between; align-items: center;">
                                <span>Audience Score</span>
                                <span>${movie.rating}%</span>
                            </div>
                            <div class="rating-bar">
                                <div class="rating-fill" style="width: ${movie.rating}%"></div>
                            </div>
                            <div class="stats-grid">
                                <div class="stat-item">
                                    <div class="stat-label">Runtime</div>
                                    <div>${movie.stats.runtime}</div>
                                </div>
                                <div class="stat-item">
                                    <div class="stat-label">Year</div>
                                    <div>${movie.stats.year}</div>
                                </div>
                                <div class="stat-item">
                                    <div class="stat-label">Director</div>
                                    <div>${movie.stats.director}</div>
                                </div>
                                <div class="stat-item">
                                    <div class="stat-label">Box Office</div>
                                    <div>${movie.stats.boxOffice}</div>
                                </div>
                            </div>
                        </div>
                    ` : ''}
                    <div class="platform-links">
                        ${platformLinks}
                    </div>
                `;
                movieList.appendChild(movieCard);
            });
        }

        function getRecommendedMovies() {
            const genre = userPreferences.genre;
            const era = userPreferences.era === 'new' || userPreferences.era === 'recent' ? 'new' : 'classic';
            const age = userPreferences.age;
            
            // Use Set to prevent duplicates
            let recommendationSet = new Set();
            
            // Helper function to add movies to Set
            const addMovies = (movies) => {
                movies.forEach(movie => {
                    // Use movie title as unique identifier
                    recommendationSet.add(JSON.stringify(movie));
                });
            };
            
            // Get primary genre recommendations
            if (movieDatabase[genre]) {
                addMovies(movieDatabase[genre][era]);
            }

            // Add kids/animation content for appropriate ages
            if (age === 'kid') {
                addMovies(movieDatabase.kids[era]);
                addMovies(movieDatabase.animation[era]);
            }

            // Mix in some other genres based on mood and preferences
            if (userPreferences.mood === 'happy') {
                addMovies(movieDatabase.comedy[era]);
            } else if (userPreferences.mood === 'excited') {
                addMovies(movieDatabase.action[era]);
            }

            // Add more variety based on selected genre
            if (genre === 'action') {
                addMovies(movieDatabase.scifi[era]);
            } else if (genre === 'drama') {
                addMovies(movieDatabase.animation[era]);
            } else if (genre === 'scifi') {
                addMovies(movieDatabase.action[era]);
            }

            // Convert Set back to array and parse the stringified movies
            let recommendations = Array.from(recommendationSet).map(movieString => JSON.parse(movieString));

            // Filter based on age
            recommendations = recommendations.filter(movie => {
                if (age === 'kid') {
                    return !movie.genre.includes('Horror') && !movie.genre.includes('Thriller');
                }
                if (age === 'teen') {
                    return !movie.genre.includes('Horror');
                }
                return true;
            });

            // Shuffle and return top 10 unique recommendations
            return shuffleArray(recommendations).slice(0, 10);
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function getPlatformIcon(platform) {
            const icons = {
                netflix: '📺',
                amazon: '🛒',
                disney: '✨',
                hulu: '📱',
                hbomax: '🎬',
                paramount: '🎥',
                youtube: '▶️',
                apple: '🍎',
                crunchyroll: '🍙',
                peacock: '🦚'
            };
            return icons[platform] || '🎬';
        }

        function getSuggestedPlatforms(genre, title) {
            // Suggest platforms based on genre and current trends
            const suggestions = [];
            
            if (genre.includes('Action') || genre.includes('Adventure')) {
                suggestions.push('Netflix', 'Prime Video');
            }
            if (genre.includes('Sci-Fi') || genre.includes('Fantasy')) {
                suggestions.push('Disney+', 'Prime Video');
            }
            if (genre.includes('Drama') || genre.includes('Romance')) {
                suggestions.push('Netflix', 'HBO Max');
            }
            if (genre.includes('Comedy')) {
                suggestions.push('Netflix', 'Hulu');
            }
            if (genre.includes('Horror') || genre.includes('Thriller')) {
                suggestions.push('Shudder', 'Prime Video');
            }

            // Get unique platforms
            const uniqueSuggestions = [...new Set(suggestions)];
            
            // Return first 2-3 suggestions
            return uniqueSuggestions.slice(0, 3).join(' or ');
        }

        // Initialize progress bar
        updateProgress();
    </script>
</body>
</html> 
