#include<iostream>
#include<SFML/Graphics.hpp>
#include<fstream>
#include<string>
#include<vector>
#include<Windows.h>
#include<algorithm>
using namespace sf;
#pragma comment(linker, "/SUBSYSTEM:windows /ENTRY:mainCRTStartup")
#include <SFML/Graphics.hpp>
#include <fstream>
#include <iostream>
#include <algorithm>

constexpr int window_width = 832, window_height = 890;
sf::RenderWindow main_window(sf::VideoMode(window_width, window_height), "Chess Puzzles", sf::Style::Titlebar | sf::Style::Close);

int board[64];
std::vector <int> answers;

void load_level(int number_of_level) {
	std::string line;
	std::ifstream level("levels/" + std::to_string(number_of_level) + ".txt");


	int i = 0;
	while (getline(level, line) && i < 64)
	{
		board[i] = std::stoi(line);
		++i;
	}

	level.close();
}

void load_level_answers(int number_of_level) {
	std::string line;
	std::ifstream level("answers/" + std::to_string(number_of_level) + ".txt");


	while (getline(level, line))
	{
		answers.push_back(std::stoi(line));
	}

	level.close();
}

int open_save_level() {
	std::string level_int;
	std::ifstream save_level("save/save.txt");
	getline(save_level, level_int);
	return std::stoi(level_int);
}

void close_save_level(int counter_of_level) {
	std::string level_int;
	std::ofstream save_level("save/save.txt");
	save_level << counter_of_level;
}


int open_save_points() {
	std::string points_int;
	std::ifstream save_points("save/points.txt");
	getline(save_points, points_int);
	return std::stoi(points_int);
}


void close_save_points(int points) {
	std::string points_int;
	std::ofstream save_points("save/points.txt");
	save_points << points;
}


void render_sprite_at_pos(sf::Sprite& sprite, int x, int y) {
	sprite.setPosition(sf::Vector2f(x, y));
	main_window.draw(sprite);
}


void render_sprites_at_situation(std::vector<std::vector<Sprite>> arr, int num_of_situation, int one_square_size, int x_offset, int y_offset) {
	for (int i = 0; i < 64; ++i) {
		if (board[i] != 0) {
			int x = i % 8 * one_square_size + x_offset;
			int y = i / 8 * one_square_size + y_offset + (window_height - window_width);
			if (board[i] > 0) {
				if (board[i] == 1) {
					render_sprite_at_pos(arr[num_of_situation][0], x, y);
				}
				else if (board[i] == 2) {
					render_sprite_at_pos(arr[num_of_situation][1], x, y);
				}
				else if (board[i] == 3) {
					render_sprite_at_pos(arr[num_of_situation][2], x, y);
				}
				else if (board[i] == 4) {
					render_sprite_at_pos(arr[num_of_situation][3], x, y);
				}
				else if (board[i] == 5) {
					render_sprite_at_pos(arr[num_of_situation][4], x, y);
				}
				else if (board[i] == 6) {
					render_sprite_at_pos(arr[num_of_situation][5], x, y);
				}
			}

			else if (board[i] < 0) {
				if (board[i] == -1) {
					render_sprite_at_pos(arr[num_of_situation][6], x, y);
				}
				else if (board[i] == -2) {
					render_sprite_at_pos(arr[num_of_situation][7], x, y);
				}
				else if (board[i] == -3) {
					render_sprite_at_pos(arr[num_of_situation][8], x, y);
				}
				else if (board[i] == -4) {
					render_sprite_at_pos(arr[num_of_situation][9], x, y);
				}
				else if (board[i] == -5) {
					render_sprite_at_pos(arr[num_of_situation][10], x, y);
				}
				else if (board[i] == -6) {
					render_sprite_at_pos(arr[num_of_situation][11], x, y);
				}
			}
		}
	}
}


int main() {
	constexpr int number_of_str = 2;
	constexpr int font_size = 60;
	constexpr int font_size_for_figures_and_board = 90;
	constexpr int one_square_size = window_width / 8;
	constexpr int one_piece_size = 150;
	constexpr float scale = one_square_size * 0.9f / one_piece_size;
	constexpr float x_offset = one_square_size * 0.1f / 2.f;
	constexpr float y_offset = one_square_size * 0.1f / 2.f;

	int square_1 = -1, square_2 = -1, i = 0, x, y, saved_piece, counter_of_moves = 0, cl_cp = 0;
	int counter_of_level = 1, points = 0, wrong_moves = 0, hint_1 = -1, hint_2 = -1;
	int square_click_1, square_click_2, number_of_variant = 0, click_copy = 9;
	bool first_move = true, correct_move = true, in_settings = false, in_settings_2 = false, first_click = false, double_click = false;
	std::string points_string = "";
	std::string board_string = "";
	std::string figures_string = "";
	sf::Time time_to_sleep;
	sf::Vector2i mouse_pos;
	sf::Event event;
	sf::Image icon;
	icon.loadFromFile("images/icon.png");
	main_window.setIcon(icon.getSize().x, icon.getSize().y, icon.getPixelsPtr());
	main_window.setFramerateLimit(60);

	sf::Font points_font;
	sf::Font board_font;
	sf::Font figures_font;
	points_font.loadFromFile("images/points_font.ttf");
	board_font.loadFromFile("images/points_font.ttf");
	figures_font.loadFromFile("images/points_font.ttf");
	sf::Text points_text(points_string, points_font, font_size);
	sf::Text board_text(board_string, board_font, font_size_for_figures_and_board);
	sf::Text figures_text(figures_string, figures_font, font_size_for_figures_and_board);

	
	sf::Texture pawn_black_variant_1;
	sf::Texture pawn_white_variant_1;
	sf::Texture knight_black_variant_1;
	sf::Texture knight_white_variant_1;
	sf::Texture bishop_black_variant_1;
	sf::Texture bishop_white_variant_1;
	sf::Texture rook_black_variant_1;
	sf::Texture rook_white_variant_1;
	sf::Texture queen_black_variant_1;
	sf::Texture queen_white_variant_1;
	sf::Texture king_black_variant_1;
	sf::Texture king_white_variant_1;

	
	sf::Texture pawn_black_variant_2;
	sf::Texture pawn_white_variant_2;
	sf::Texture knight_black_variant_2;
	sf::Texture knight_white_variant_2;
	sf::Texture bishop_black_variant_2;
	sf::Texture bishop_white_variant_2;
	sf::Texture rook_black_variant_2;
	sf::Texture rook_white_variant_2;
	sf::Texture queen_black_variant_2;
	sf::Texture queen_white_variant_2;
	sf::Texture king_black_variant_2;
	sf::Texture king_white_variant_2;


	sf::Texture pawn_black_variant_3;
	sf::Texture pawn_white_variant_3;
	sf::Texture knight_black_variant_3;
	sf::Texture knight_white_variant_3;
	sf::Texture bishop_black_variant_3;
	sf::Texture bishop_white_variant_3;
	sf::Texture rook_black_variant_3;
	sf::Texture rook_white_variant_3;
	sf::Texture queen_black_variant_3;
	sf::Texture queen_white_variant_3;
	sf::Texture king_black_variant_3;
	sf::Texture king_white_variant_3;

	
	sf::Texture pawn_black_variant_4;
	sf::Texture pawn_white_variant_4;
	sf::Texture knight_black_variant_4;
	sf::Texture knight_white_variant_4;
	sf::Texture bishop_black_variant_4;
	sf::Texture bishop_white_variant_4;
	sf::Texture rook_black_variant_4;
	sf::Texture rook_white_variant_4;
	sf::Texture queen_black_variant_4;
	sf::Texture queen_white_variant_4;
	sf::Texture king_black_variant_4;
	sf::Texture king_white_variant_4;


	sf::Texture chess_board_texture;
	sf::Texture hint;
	sf::Texture solve;
	sf::Texture settings;
	sf::Texture settings_menu;
	sf::Texture out_door;
	sf::Texture r_arrow;
	sf::Texture l_arrow;
	


	pawn_black_variant_1.loadFromFile("images/pawn_black_variant_1.png");
	pawn_white_variant_1.loadFromFile("images/pawn_white_variant_1.png");
	knight_black_variant_1.loadFromFile("images/knight_black_variant_1.png");
	knight_white_variant_1.loadFromFile("images/knight_white_variant_1.png");
	bishop_black_variant_1.loadFromFile("images/bishop_black_variant_1.png");
	bishop_white_variant_1.loadFromFile("images/bishop_white_variant_1.png");
	rook_black_variant_1.loadFromFile("images/rook_black_variant_1.png");
	rook_white_variant_1.loadFromFile("images/rook_white_variant_1.png");
	queen_black_variant_1.loadFromFile("images/queen_black_variant_1.png");
	queen_white_variant_1.loadFromFile("images/queen_white_variant_1.png");
	king_black_variant_1.loadFromFile("images/king_black_variant_1.png");
	king_white_variant_1.loadFromFile("images/king_white_variant_1.png");

	pawn_black_variant_2.loadFromFile("images/pawn_black_variant_2.png");
	pawn_white_variant_2.loadFromFile("images/pawn_white_variant_2.png");
	knight_black_variant_2.loadFromFile("images/knight_black_variant_2.png");
	knight_white_variant_2.loadFromFile("images/knight_white_variant_2.png");
	bishop_black_variant_2.loadFromFile("images/bishop_black_variant_2.png");
	bishop_white_variant_2.loadFromFile("images/bishop_white_variant_2.png");
	rook_black_variant_2.loadFromFile("images/rook_black_variant_2.png");
	rook_white_variant_2.loadFromFile("images/rook_white_variant_2.png");
	queen_black_variant_2.loadFromFile("images/queen_black_variant_2.png");
	queen_white_variant_2.loadFromFile("images/queen_white_variant_2.png");
	king_black_variant_2.loadFromFile("images/king_black_variant_2.png");
	king_white_variant_2.loadFromFile("images/king_white_variant_2.png");


	pawn_black_variant_3.loadFromFile("images/pawn_black_variant_3.png");
	pawn_white_variant_3.loadFromFile("images/pawn_white_variant_3.png");
	knight_black_variant_3.loadFromFile("images/knight_black_variant_3.png");
	knight_white_variant_3.loadFromFile("images/knight_white_variant_3.png");
	bishop_black_variant_3.loadFromFile("images/bishop_black_variant_3.png");
	bishop_white_variant_3.loadFromFile("images/bishop_white_variant_3.png");
	rook_black_variant_3.loadFromFile("images/rook_black_variant_3.png");
	rook_white_variant_3.loadFromFile("images/rook_white_variant_3.png");
	queen_black_variant_3.loadFromFile("images/queen_black_variant_3.png");
	queen_white_variant_3.loadFromFile("images/queen_white_variant_3.png");
	king_black_variant_3.loadFromFile("images/king_black_variant_3.png");
	king_white_variant_3.loadFromFile("images/king_white_variant_3.png");


	pawn_black_variant_4.loadFromFile("images/pawn_black_variant_4.png");
	pawn_white_variant_4.loadFromFile("images/pawn_white_variant_4.png");
	knight_black_variant_4.loadFromFile("images/knight_black_variant_4.png");
	knight_white_variant_4.loadFromFile("images/knight_white_variant_4.png");
	bishop_black_variant_4.loadFromFile("images/bishop_black_variant_4.png");
	bishop_white_variant_4.loadFromFile("images/bishop_white_variant_4.png");
	rook_black_variant_4.loadFromFile("images/rook_black_variant_4.png");
	rook_white_variant_4.loadFromFile("images/rook_white_variant_4.png");
	queen_black_variant_4.loadFromFile("images/queen_black_variant_4.png");
	queen_white_variant_4.loadFromFile("images/queen_white_variant_4.png");
	king_black_variant_4.loadFromFile("images/king_black_variant_4.png");
	king_white_variant_4.loadFromFile("images/king_white_variant_4.png");


	chess_board_texture.loadFromFile("images/board_chess.jpg");
	hint.loadFromFile("images/hint.png");
	solve.loadFromFile("images/solve_green.png");
	settings.loadFromFile("images/settings.png");
	settings_menu.loadFromFile("images/settings_menu.jpeg");
	out_door.loadFromFile("images/out_door.png");
	r_arrow.loadFromFile("images/right_arrow.png");
	l_arrow.loadFromFile("images/left_arrow.png");


	sf::Sprite pawn_wh_1(pawn_white_variant_1);
	sf::Sprite pawn_bl_1(pawn_black_variant_1);
	sf::Sprite knight_wh_1(knight_white_variant_1);
	sf::Sprite knight_bl_1(knight_black_variant_1);
	sf::Sprite bishop_wh_1(bishop_white_variant_1);
	sf::Sprite bishop_bl_1(bishop_black_variant_1);
	sf::Sprite rook_wh_1(rook_white_variant_1);
	sf::Sprite rook_bl_1(rook_black_variant_1);
	sf::Sprite queen_wh_1(queen_white_variant_1);
	sf::Sprite queen_bl_1(queen_black_variant_1);
	sf::Sprite king_wh_1(king_white_variant_1);
	sf::Sprite king_bl_1(king_black_variant_1);


	sf::Sprite pawn_wh_2(pawn_white_variant_2);
	sf::Sprite pawn_bl_2(pawn_black_variant_2);
	sf::Sprite knight_wh_2(knight_white_variant_2);
	sf::Sprite knight_bl_2(knight_black_variant_2);
	sf::Sprite bishop_wh_2(bishop_white_variant_2);
	sf::Sprite bishop_bl_2(bishop_black_variant_2);
	sf::Sprite rook_wh_2(rook_white_variant_2);
	sf::Sprite rook_bl_2(rook_black_variant_2);
	sf::Sprite queen_wh_2(queen_white_variant_2);
	sf::Sprite queen_bl_2(queen_black_variant_2);
	sf::Sprite king_wh_2(king_white_variant_2);
	sf::Sprite king_bl_2(king_black_variant_2);


	sf::Sprite pawn_wh_3(pawn_white_variant_3);
	sf::Sprite pawn_bl_3(pawn_black_variant_3);
	sf::Sprite knight_wh_3(knight_white_variant_3);
	sf::Sprite knight_bl_3(knight_black_variant_3);
	sf::Sprite bishop_wh_3(bishop_white_variant_3);
	sf::Sprite bishop_bl_3(bishop_black_variant_3);
	sf::Sprite rook_wh_3(rook_white_variant_3);
	sf::Sprite rook_bl_3(rook_black_variant_3);
	sf::Sprite queen_wh_3(queen_white_variant_3);
	sf::Sprite queen_bl_3(queen_black_variant_3);
	sf::Sprite king_wh_3(king_white_variant_3);
	sf::Sprite king_bl_3(king_black_variant_3);


	sf::Sprite pawn_wh_4(pawn_white_variant_4);
	sf::Sprite pawn_bl_4(pawn_black_variant_4);
	sf::Sprite knight_wh_4(knight_white_variant_4);
	sf::Sprite knight_bl_4(knight_black_variant_4);
	sf::Sprite bishop_wh_4(bishop_white_variant_4);
	sf::Sprite bishop_bl_4(bishop_black_variant_4);
	sf::Sprite rook_wh_4(rook_white_variant_4);
	sf::Sprite rook_bl_4(knight_white_variant_4);
	sf::Sprite queen_wh_4(queen_white_variant_4);
	sf::Sprite queen_bl_4(queen_black_variant_4);
	sf::Sprite king_wh_4(king_white_variant_4);
	sf::Sprite king_bl_4(king_black_variant_4);


	sf::Sprite chess_hint(hint);
	sf::Sprite chess_solve(solve);
	sf::Sprite chess_board(chess_board_texture);
	sf::Sprite chess_settings(settings);
	sf::Sprite chess_settings_menu(settings_menu);
	sf::Sprite chess_out_door(out_door);
	sf::Sprite right_arrow(r_arrow);
	sf::Sprite left_arrow(l_arrow);
	chess_board.setPosition(sf::Vector2f(0, window_height - window_width));
	chess_board.setScale(sf::Vector2f((float)window_width / chess_board_texture.getSize().x, (float)window_width / chess_board_texture.getSize().y));

	
	pawn_bl_1.setScale(sf::Vector2f(scale, scale));
	pawn_wh_1.setScale(sf::Vector2f(scale, scale));
	knight_bl_1.setScale(sf::Vector2f(scale, scale));
	knight_wh_1.setScale(sf::Vector2f(scale, scale));
	rook_bl_1.setScale(sf::Vector2f(scale, scale));
	rook_wh_1.setScale(sf::Vector2f(scale, scale));
	bishop_bl_1.setScale(sf::Vector2f(scale, scale));
	bishop_wh_1.setScale(sf::Vector2f(scale, scale));
	queen_bl_1.setScale(sf::Vector2f(scale, scale));
	queen_wh_1.setScale(sf::Vector2f(scale, scale));
	king_bl_1.setScale(sf::Vector2f(scale, scale));
	king_wh_1.setScale(sf::Vector2f(scale, scale));


	pawn_bl_2.setScale(sf::Vector2f(scale, scale));
	pawn_wh_2.setScale(sf::Vector2f(scale, scale));
	knight_bl_2.setScale(sf::Vector2f(scale, scale));
	knight_wh_2.setScale(sf::Vector2f(scale, scale));
	rook_bl_2.setScale(sf::Vector2f(scale, scale));
	rook_wh_2.setScale(sf::Vector2f(scale, scale));
	bishop_bl_2.setScale(sf::Vector2f(scale, scale));
	bishop_wh_2.setScale(sf::Vector2f(scale, scale));
	queen_bl_2.setScale(sf::Vector2f(scale, scale));
	queen_wh_2.setScale(sf::Vector2f(scale, scale));
	king_bl_2.setScale(sf::Vector2f(scale, scale));
	king_wh_2.setScale(sf::Vector2f(scale, scale));


	pawn_bl_3.setScale(sf::Vector2f(scale, scale));
	pawn_wh_3.setScale(sf::Vector2f(scale, scale));
	knight_bl_3.setScale(sf::Vector2f(scale, scale));
	knight_wh_3.setScale(sf::Vector2f(scale, scale));
	rook_bl_3.setScale(sf::Vector2f(scale, scale));
	rook_wh_3.setScale(sf::Vector2f(scale, scale));
	bishop_bl_3.setScale(sf::Vector2f(scale, scale));
	bishop_wh_3.setScale(sf::Vector2f(scale, scale));
	queen_bl_3.setScale(sf::Vector2f(scale, scale));
	queen_wh_3.setScale(sf::Vector2f(scale, scale));
	king_bl_3.setScale(sf::Vector2f(scale, scale));
	king_wh_3.setScale(sf::Vector2f(scale, scale));


	pawn_bl_4.setScale(sf::Vector2f(scale, scale));
	pawn_wh_4.setScale(sf::Vector2f(scale, scale));
	knight_bl_4.setScale(sf::Vector2f(scale, scale));
	knight_wh_4.setScale(sf::Vector2f(scale, scale));
	rook_bl_4.setScale(sf::Vector2f(scale, scale));
	rook_wh_4.setScale(sf::Vector2f(scale, scale));
	bishop_bl_4.setScale(sf::Vector2f(scale, scale));
	bishop_wh_4.setScale(sf::Vector2f(scale, scale));
	queen_bl_4.setScale(sf::Vector2f(scale, scale));
	queen_wh_4.setScale(sf::Vector2f(scale, scale));
	king_bl_4.setScale(sf::Vector2f(scale, scale));
	king_wh_4.setScale(sf::Vector2f(scale, scale));


	std::vector<Sprite> figures_variant_1 = { pawn_wh_1,knight_wh_1,bishop_wh_1,
	rook_wh_1,queen_wh_1,king_wh_1};
	std::vector<Sprite> figures_variant_2 = {pawn_wh_2,knight_wh_2,bishop_wh_2,
	rook_wh_2,queen_wh_2, king_wh_2}; 
	std::vector<Sprite> figures_variant_3 = { pawn_wh_3,knight_wh_3,bishop_wh_3,
	rook_wh_3,queen_wh_3, king_wh_3 };
	std::vector<Sprite> figures_variant_4 = { pawn_wh_4,knight_wh_4,bishop_wh_4,
	rook_wh_4,queen_wh_4, king_wh_4 };
	std::vector<std::vector<Sprite>> all_figures = { {pawn_wh_1,knight_wh_1,bishop_wh_1,
	rook_wh_1,queen_wh_1,king_wh_1, pawn_bl_1,knight_bl_1,bishop_bl_1,
	rook_bl_1,queen_bl_1,king_bl_1 }, {pawn_wh_2,knight_wh_2,bishop_wh_2,
	rook_wh_2,queen_wh_2, king_wh_2,  pawn_bl_2,knight_bl_2,bishop_bl_2,
	rook_bl_2,queen_bl_2, king_bl_2}, {pawn_wh_3,knight_wh_3,bishop_wh_3,
	rook_wh_3,queen_wh_3,king_wh_3, pawn_bl_3,knight_bl_3,bishop_bl_3,
	rook_bl_3,queen_bl_3,king_bl_3}, {pawn_wh_4,knight_wh_4,bishop_wh_4,
	rook_wh_4,queen_wh_4,king_wh_4, pawn_bl_4,knight_bl_4,bishop_bl_4,
	rook_bl_4,queen_bl_4,king_bl_4 } };
	
	points = open_save_points();
	counter_of_level = open_save_level();
	load_level(counter_of_level);
	load_level_answers(counter_of_level);

	while (main_window.isOpen()) {

		if (square_1 != -1 && square_2 != -1) {
			if (!correct_move) {
				board[square_1] = board[square_2];
				board[square_2] = saved_piece;
				wrong_moves++;
				time_to_sleep = sf::milliseconds(400);
				sf::sleep(time_to_sleep);
				goto reset_tiles;
			}
			time_to_sleep = sf::milliseconds(400);
			sf::sleep(time_to_sleep);

			if ((counter_of_moves + 1) * 4 > answers.size() - 1) {
				counter_of_level++;
				answers.clear();
				counter_of_moves = 0;
				points += std::max(1000 - wrong_moves * 100, 0);
				load_level(counter_of_level);
				load_level_answers(counter_of_level);
				goto reset_tiles;
			}

			board[answers[4 * counter_of_moves + 3]] = board[answers[4 * counter_of_moves + 2]];
			board[answers[4 * counter_of_moves + 2]] = 0;
			counter_of_moves++;
		reset_tiles:
			square_1 = -1;
			square_2 = -1;
			hint_1 = -1;
			hint_2 = -1;
		}

		while (main_window.pollEvent(event)) {
			mouse_pos = sf::Mouse::getPosition(main_window);
			if (event.type == sf::Event::Closed) {
				main_window.close();
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && mouse_pos.x <= 60 && mouse_pos.y <= 60 && in_settings == true) {
				in_settings = false;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && mouse_pos.x <= 50 && mouse_pos.y <= 50) {
				in_settings = true;
				in_settings_2 = false;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && in_settings && mouse_pos.y >= 60) {
				first_click = true;
				square_click_1 = (mouse_pos.x / one_square_size) + ((mouse_pos.y - (window_height - window_width)) / one_square_size) * 8;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && in_settings == true && mouse_pos.x >= window_width - 73 && mouse_pos.y <= 50) {
				in_settings_2 = true;
				in_settings = false;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && in_settings == true && mouse_pos.x <= 73 && mouse_pos.y >= 50) {
				in_settings = true;
				in_settings_2 = false;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && in_settings_2 == true && mouse_pos.y >= 60) {
				first_click = true;
				square_click_2 = (mouse_pos.x / one_square_size) + ((mouse_pos.y - (window_height - window_width)) / one_square_size) * 8;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && mouse_pos.x <= 105 && mouse_pos.y <= 50) {
				if (points >= 1000 && hint_1 == -1 && hint_2 == -1) {
					points -= 1000;
					hint_1 = answers[counter_of_moves * 4];
					hint_2 = answers[counter_of_moves * 4 + 1];
				}
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && first_move) {
				square_1 = (mouse_pos.x / one_square_size) + ((mouse_pos.y - (window_height - window_width)) / one_square_size) * 8;
				first_move = false;
			}
			else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left && !first_move) {
				square_2 = (mouse_pos.x / one_square_size) + ((mouse_pos.y - (window_height - window_width)) / one_square_size) * 8;
				first_move = true;
			}
		}
			if (square_1 != -1 && square_2 != -1) {
				saved_piece = board[square_2];
				board[square_2] = board[square_1];
				board[square_1] = 0;
				correct_move = square_1 == answers[counter_of_moves * 4] && square_2 == answers[counter_of_moves * 4 + 1];
			}
		
		main_window.clear(Color(250, 220, 100, 0));

		if (in_settings == true) {
			render_sprite_at_pos(chess_settings_menu, 0, 0);
			render_sprite_at_pos(chess_out_door, 5,5);
			render_sprite_at_pos(right_arrow, window_width - 73, 0);
			int j = 9;
			int x_1 = j % 8 * one_square_size + x_offset;
			int y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
			if (first_click) {
				click_copy = square_click_1;
				x_1 = square_click_1 % 8 * one_square_size + x_offset;
				y_1 = square_click_1 / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(chess_solve, x_1, y_1);
			}
			for (int i = 0; i < 6; i++) {
				x_1 = j % 8 * one_square_size + x_offset;
				y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(figures_variant_1[i], x_1, y_1);
				j++;
			}
			j = 25;
			for (int i = 0; i < 6; i++) {
				x_1 = j % 8 * one_square_size + x_offset;
				y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(figures_variant_2[i], x_1, y_1);
				j++;
			}
			j = 41;
			for (int i = 0; i < 6; i++) {
				x_1 = j % 8 * one_square_size + x_offset;
				y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(figures_variant_3[i], x_1, y_1);
				j++;
			}
			if (click_copy >= 9 && click_copy <= 15) {
				number_of_variant = 0;
			}
			else if (click_copy >= 25 && click_copy <= 31) {
				number_of_variant = 1;
			}
			else if (click_copy >= 41 && click_copy <= 46) {
				number_of_variant = 2;
			}
		}
		else if (in_settings_2 == true) {
			render_sprite_at_pos(chess_settings_menu, 0, 0);
			render_sprite_at_pos(left_arrow, 0, 0);
			render_sprite_at_pos(right_arrow, window_width - 73, 0);
			int j = 9;
			int x_1 = j % 8 * one_square_size + x_offset;
			int y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
			if (first_click) {
				click_copy = square_click_1;
				cl_cp = click_copy + 64;
				x_1 = square_click_1 % 8 * one_square_size + x_offset;
				y_1 = square_click_1 / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(chess_solve, x_1, y_1);
			}
			for (int i = 0; i < 6; i++) {
				x_1 = j % 8 * one_square_size + x_offset;
				y_1 = j / 8 * one_square_size + y_offset + (window_height - window_width);
				render_sprite_at_pos(figures_variant_4[i], x_1, y_1);
				j++;
			}
			if (cl_cp >= 73 && cl_cp <= 79) {
				number_of_variant = 4;
			}
		}
		else {
			first_click = false;
			main_window.draw(chess_board);
			render_sprite_at_pos(chess_settings, 5, 5);
			render_sprite_at_pos(chess_hint, 55, 3);

			if (hint_1 != -1) {
				x = hint_1 % 8 * one_square_size;
				y = hint_1 / 8 * one_square_size + (window_height - window_width);
				render_sprite_at_pos(chess_solve, x, y);
				x = hint_2 % 8 * one_square_size;
				y = hint_2 / 8 * one_square_size + (window_height - window_width);
				render_sprite_at_pos(chess_solve, x, y);
			}

			render_sprites_at_situation(all_figures, number_of_variant, one_square_size, x_offset, y_offset);

			points_string = std::to_string(points);
			points_text.setString(points_string);

			points_text.setPosition(sf::Vector2f(
				(window_width / 2 - points_text.getLocalBounds().left / 2) - points_text.getGlobalBounds().width / 2.f,
				-(points_text.getLocalBounds().top / 2)));

			main_window.draw(points_text);
		}
		main_window.display();
	}
	close_save_level(counter_of_level);
	close_save_points(points);
}
