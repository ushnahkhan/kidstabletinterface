#include <SFML/Graphics.hpp>
#include <locale>
#include <iostream>
#include <fstream>
#include <cstring>
#include <math.h>
using namespace std;
sf::ConvexShape triangle;
sf::CircleShape circle;
sf::RectangleShape rectangle;

bool isdigit(char c)
{
	if (c >= '0' && c <= '9')
		return true;
	else
		return false;
}

bool compare1(char array1[], char commandarray[], int length)   //compares two strings till the given index
{
	int flag = 0;
	int found = 0;
	for (int i = 0; i < length && flag == 0; i++)
	{
		if (commandarray[i] != array1[i])
			flag = 1;
	}
	if (flag == 0)
		return true;
	else
		return false;
}

bool compare2(char array1[], char commandarray[])  //strcmp used for pd pu cs
{
	int flag = 0;
	int found = 0;
	int size = 0;
	int size1 = 0;
	while (array1[size] != 0)
		size++;
	while (commandarray[size1] != 0)
		size1++;
	if (size1 != size)
		return false;

	for (int i = 0; array1[i] != 0 && flag == 0; i++)
	{
		if (commandarray[i] != array1[i])
			flag = 1;
	}
	if (flag == 0)
		return true;
	else
		return false;
}




bool compall(char str[], char ford[], int length, char error[], float &steps)   //for fd bk forward backward width circle (put command in str[] and the command you want to check in ford[] and its length in length)
{
	int flag = 1;
	steps = 0.0;
	int z = length;
	if (compare1(str, ford, length))
	{
		while (str[z] == ' ')
			z++;
		if (str[z] == 0)
		{
			error[0] = 'I'; error[1] = 'n'; error[2] = 'c'; error[3] = 'o';
			error[4] = 'm'; error[5] = 'p'; error[6] = 'l'; error[7] = 'e';
			error[8] = 't'; error[9] = 'e'; error[10] = ' '; error[11] = 'c';
			error[12] = 'o'; error[13] = 'm'; error[14] = 'm'; error[15] = 'a';
			error[16] = 'n'; error[17] = 'd'; error[18] = 0;
			return false;
		}
		for (int i = z; str[i] != 0 && flag == 1; i++)
		{
			if (isdigit(str[i]))
			{
				steps = (steps * 10.0) + (str[i] - 48.0);
				flag = 1;
			}
			else
				flag = 0;
		}
		if (flag == 1)
			return true;
		else
			return false;
	}
	else
		return false;
}

bool color(char command[], char color1[], char error[])  //for colors
{
	int length = 5;
	int i = 0;
	color1[0] = 0;
	if (compare1(command, "color", 5))
	{
		while (command[length] == ' ')
			length++;
		int z = length;
		if (command[z] >= 'A'&&command[z] <= 'Z')
		{
			while (command[z] >= 'A'&&command[z] <= 'Z')
			{
				color1[i] = command[z];
				i++;
				z++;
			}
			color1[i] = 0;
		}
		else
		{
			return false;
		}

		if (command[z] == 0)
		{
			if (compare2(color1, "AQUA") || compare2(color1, "WHITE") || compare2(color1, "YELLOW") || compare2(color1, "RED") || compare2(color1, "ORANGE") || compare2(color1, "BLUE") || compare2(color1, "GREY") || compare2(color1, "GREEN") || compare2(color1, "PURPLE") || compare2(color1, "BROWN"))
				;
			else
			{
				error[0] = 'I'; error[1] = 'n'; error[2] = 'c'; error[3] = 'o';
				error[4] = 'm'; error[5] = 'p'; error[6] = 'l'; error[7] = 'e';
				error[8] = 't'; error[9] = 'e'; error[10] = ' '; error[11] = 'c';
				error[12] = 'o'; error[13] = 'm'; error[14] = 'm'; error[15] = 'a';
				error[16] = 'n'; error[17] = 'd'; error[18] = 0;
				return false;
			}
		}
		else
			return false;
	}
	else
		return false;
	return true;
}



bool rotate1(char str[], char command[], char err[], int &angle)
{
	angle = 0;
	int position = 2;
	if (compare1(str, command, 2))
	{
		while (str[position] == ' ')
		{
			position++;
		}

		if (str[position] == 0)
		{
			err[0] = 'I'; err[1] = 'n'; err[2] = 'c'; err[3] = 'o';
			err[4] = 'm'; err[5] = 'p'; err[6] = 'l'; err[7] = 'e';
			err[8] = 't'; err[9] = 'e'; err[10] = ' '; err[11] = 'c';
			err[12] = 'o'; err[13] = 'm'; err[14] = 'm'; err[15] = 'a';
			err[16] = 'n'; err[17] = 'd'; err[18] = 0;
			return false;
		}
		while (str[position] >= '0'&&str[position] <= '9')
		{
			angle = (angle * 10);
			angle = angle + (str[position] - 48);
			position++;
		}
		if (str[position] != 0)
			return false;
		if (angle % 30 != 0 || angle % 45 != 0)
			return false;
	}
	else
		return false;
	return true;
}




bool repeat1(char str[], int &n)  //checks every possibilty of invalidity except commands in brakcet
{
	int length = 6;
	n = 0;
	if (compare1(str, "repeat", 6))
	{
		while (str[length] == ' ')
		{
			length++;
		}
		if (isdigit(str[length]))
		{
			while (isdigit(str[length]) == true)
			{
				n = (n * 10);
				n = n + (str[length] - 48);
				length++;
			}
			if (str[length] != 91)
				return false;
			while (str[length] != 0)
				length++;
			if (str[length - 1] != 93)
				return false;
		}
		else
			return false;
		return true;
	}
	else
		return false;

}


bool repeatval(char str[], int &n, float operand[100], char commands[][100], char colors[][100], char func[][100], char err[], int &noofcommands)  //checks validity of commands in bracket. 2d array for function and integers is also made
{
	int error = 0;
	int flag = 0;
	int startindex = 0;
	int endindex = 0;
	int x = 0;
	int y = 0;
	int z = 0;
	if (repeat1(str, n))
	{
		for (int i = 6; str[i] != 0; i++)
		{
			if (str[i] == 91)
				startindex = i+1;
			if (str[i] == 93)
				endindex = i-1;
		}
		for (int i = startindex; i <= endindex; i++)
		{
			if (str[i] == ' ' && (isdigit(str[i - 1])||(str[i-1]>='A'&&str[i-1]<='Z')))
			{
				commands[x][y] = 0;
				x++;
				y = 0;
				i++;
			}
			commands[x][y] = str[i];
			y++;
		}
		commands[x][y] = 0;
		x++;
		noofcommands = x;
		int a = 0, b = 0, c = 0, ci = 0, e = 0;
		for (int i = 0; i < x; i++, a++, c++)
		{
			b = 0;
			for (int j = 0; commands[i][j] != 0; j++)
			{

				if (isdigit(commands[i][j]))
				{
					operand[c] = (operand[c] * 10) + (commands[i][j] - 48);
				}
				else if (commands[i][j] >= 'a' && commands[i][j] <= 'z')
				{
					func[a][b] = commands[i][j];
					b++;
				}
				else if (commands[i][j] >= 'A'&& commands[i][j] <= 'Z')
				{
					e = 0;
					while (commands[i][j] >= 'A'&& commands[i][j] <= 'Z')
					{
						colors[ci][e] = commands[i][j];
						e++;
						j++;
					}
					c++;
					ci++;
				}
			}
		}
		int colorindex = 0,num;
		for (int i = 0; i < x; i++)
		{
			num = (int)operand[i];
			if (compall(commands[i], "fd", 2, err, operand[i]) || compall(commands[i], "bk", 2, err, operand[i]) || compall(commands[i], "forward", 7, err, operand[i]) ||
				compall(commands[i], "backward", 8, err, operand[i]) || compall(commands[i], "width", 5, err, operand[i]) || compall(commands[i], "circle", 6, err, operand[i]) ||
				rotate1(commands[i], "rt", err, num) || rotate1(commands[i], "lt", err, num) || compare2(commands[i], "cs") || compare2(commands[i], "pu") || compare2(commands[i], "pd") || 
				color(commands[i], colors[colorindex], err))
			{
				if (color(commands[i], colors[colorindex], err))
					colorindex++;
			}
			else
				return false;
		}
		return true;
	}
	else
		return false;
}
bool filework(char str[], char command[], char file[],char err[])   // for load and save
{
	if (compare1(str, command,4))
	{
		int length = 4;
		int n = 0;
		if (str[length] == ' ')
		{
			while (str[length] == ' ')
			{
				length++;
			}
		}
		else
			return false;
		if (str[length] != 34)
		{
			err[0] = 'I'; err[1] = 'n'; err[2] = 'c'; err[3] = 'o';
			err[4] = 'm'; err[5] = 'p'; err[6] = 'l'; err[7] = 'e';
			err[8] = 't'; err[9] = 'e'; err[10] = ' '; err[11] = 'c';
			err[12] = 'o'; err[13] = 'm'; err[14] = 'm'; err[15] = 'a';
			err[16] = 'n'; err[17] = 'd'; err[18] = 0;
			return false;
		}
		int a = 0;
		while (str[length] != 0)
		{
			file[a] = str[length];
			a++;
			length++;
		}
		file[a] =0;
		if (file[a - 1] != 34)
		{
			err[0] = 'I'; err[1] = 'n'; err[2] = 'c'; err[3] = 'o';
			err[4] = 'm'; err[5] = 'p'; err[6] = 'l'; err[7] = 'e';
			err[8] = 't'; err[9] = 'e'; err[10] = ' '; err[11] = 'c';
			err[12] = 'o'; err[13] = 'm'; err[14] = 'm'; err[15] = 'a';
			err[16] = 'n'; err[17] = 'd'; err[18] = 0;
			return false;
		}
		if (file[a - 2] != 't' || file[a- 3] != 'x' || file[a - 4] != 't'|| file[a - 5] != '.')
		{
			err[0] = 'I'; err[1] = 'n'; err[2] = 'c'; err[3] = 'o';
			err[4] = 'm'; err[5] = 'p'; err[6] = 'l'; err[7] = 'e';
			err[8] = 't'; err[9] = 'e'; err[10] = ' '; err[11] = 'c';
			err[12] = 'o'; err[13] = 'm'; err[14] = 'm'; err[15] = 'a';
			err[16] = 'n'; err[17] = 'd'; err[18] = 0;
			return false;
		}
		return true;
	}
	else
		return false;
}
void forward(float lines[][8], int &index, float steps, int penup, int rbits, int gbits, int bbits, float thickness)
{
	sf::Transform transform = triangle.getTransform();
	sf::Vector2f position0 = transform.transformPoint(triangle.getPoint(0));
	sf::Vector2f position1 = transform.transformPoint(triangle.getPoint(1));
	sf::Vector2f position2 = transform.transformPoint(triangle.getPoint(2));
	if ((position0.x<position2.x) && (position0.x>position1.x))
	{
		lines[index][0] = position0.x - (thickness / 2.0);
		lines[index][1] = position0.y - steps ;
		lines[index][2] = thickness;
		lines[index][3] = steps;


		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(0.0, -steps);
	}
	else if ((position1.x == position2.x) && (position0.x > position2.x))
	{
		lines[index][0] = position0.x;
		lines[index][1] = position0.y - (thickness / 2.0);
		lines[index][2] = steps;
		lines[index][3] = thickness;
		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(steps, 0.0);
	}
	else if ((position0.x > position2.x) && (position0.x < position1.x))
	{
		lines[index][0] = (position0.x - (thickness / 2.0));
		lines[index][1] = position0.y;
		lines[index][2] = thickness;
		lines[index][3] = steps;

		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(0.0, steps);
	}
	else if ((position0.x < position1.x) && (position1.x == position2.x))
	{
		lines[index][0] = position0.x-steps;
		lines[index][1] = position0.y - (thickness / 2.0);
		lines[index][2] = steps;
		lines[index][3] = thickness;
		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(-steps, 0.0);
	}


}

void backward(float lines[][8], int &index, float steps, int penup, int rbits, int gbits, int bbits, float thickness)
{
	sf::Transform transform = triangle.getTransform();
	sf::Vector2f position0 = transform.transformPoint(triangle.getPoint(0));
	sf::Vector2f position1 = transform.transformPoint(triangle.getPoint(1));
	sf::Vector2f position2 = transform.transformPoint(triangle.getPoint(2));
	if ((position0.x<position2.x) && (position0.x>position1.x))
	{
		lines[index][0] = position0.x - (thickness / 2.0);
		lines[index][1] = position0.y;
		lines[index][2] = thickness;
		lines[index][3] = steps;


		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(0.0, steps);

	}
	else if ((position1.x == position2.x) && (position0.x > position2.x))
	{

		lines[index][0] = position0.x - steps;
		lines[index][1] = position0.y - (thickness / 2.0);
		lines[index][2] = steps;
		lines[index][3] = thickness;


		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(-steps, 0.0);
	}
	else if ((position0.x > position2.x) && (position0.x < position1.x))
	{

		lines[index][0] = position0.x - (thickness / 2.0);
		lines[index][1] = position0.y - steps;
		lines[index][2] = thickness;
		lines[index][3] = steps;


		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(0.0, -steps);
	}
	else if ((position0.x < position1.x) && (position1.x == position2.x))
	{
		lines[index][0] = position0.x ;
		lines[index][1] = position0.y - (thickness / 2.0);
		lines[index][2] = steps;
		lines[index][3] = thickness;


		if (penup == 0)
		{
			lines[index][4] = rbits;
			lines[index][5] = gbits;
			lines[index][6] = bbits;
		}
		else if (penup == 1)
		{
			lines[index][4] = 0.0;
			lines[index][5] = 0.0;
			lines[index][6] = 0.0;
		}
		lines[index][7] = penup;
		index++;
		triangle.move(steps, 0.0);
	}


}
void right(int angle)
{
	sf::Transform transform = triangle.getTransform();
	sf::Vector2f position0 = triangle.getPoint(0);
	sf::Vector2f position1 = triangle.getPoint(1);
	sf::Vector2f position2 = triangle.getPoint(2);
	float x, y;
	if (position1.x == position2.x)
	{
		triangle.setOrigin(position2.x, position0.y);
		x = transform.transformPoint(triangle.getPoint(0)).x;
		y = transform.transformPoint(triangle.getPoint(0)).y;
	}
	else
	{
		triangle.setOrigin(position0.x, position0.y);
		x = transform.transformPoint(triangle.getPoint(0)).x;
		y = transform.transformPoint(triangle.getPoint(0)).y;
	}
	triangle.rotate(angle);
	triangle.setPosition(x, y);


}
void left(int angle)
{
	sf::Transform transform = triangle.getTransform();
	sf::Vector2f position0 = triangle.getPoint(0);
	sf::Vector2f position1 = triangle.getPoint(1);
	sf::Vector2f position2 = triangle.getPoint(2);
	float x, y;
	if (position1.x == position2.x)
	{
		triangle.setOrigin(position2.x, position0.y);
		x = transform.transformPoint(triangle.getPoint(0)).x;
		y = transform.transformPoint(triangle.getPoint(0)).y;
	}
	else
	{
		triangle.setOrigin(position0.x, position0.y);
		x = transform.transformPoint(triangle.getPoint(0)).x;
		y = transform.transformPoint(triangle.getPoint(0)).y;
	}
	triangle.rotate(-angle);
	triangle.setPosition(x, y);

}
void selectcolor(char color[], int &rbits, int &gbits, int &bbits)
{
	
		if (color[0] == 'A'&&color[1] == 'Q'&&color[2] == 'U'&&color[3] == 'A')
		{
			rbits = 0;
			gbits = 255;
			bbits = 255;
		}
		else if (color[0] == 'W'&&color[1] == 'H'&&color[2] == 'I'&&color[3] == 'T'&&color[4] == 'E')
		{
			rbits = 255;
			gbits = 255;
			bbits = 255;
		}
		else if (color[0] == 'Y'&&color[1] == 'E'&&color[2] == 'L'&&color[3] == 'L'&&color[4] == 'O'&&color[5] == 'W')
		{
			rbits = 255;
			gbits = 255;
			bbits = 0;
		}
		else if (color[0] == 'R'&&color[1] == 'E'&&color[2] == 'D')
		{
			rbits = 255;
			gbits = 0;
			bbits = 0;
		}
		else if (color[0] == 'O'&&color[1] == 'R'&&color[2] == 'A'&&color[3] == 'N'&&color[4] == 'G'&&color[5] == 'E')
		{
			rbits = 255;
			gbits = 165;
			bbits = 0;
		}
		else if (color[0] == 'B'&&color[1] == 'L'&&color[2] == 'U'&&color[3] == 'E')
		{
			rbits = 0;
			gbits = 0;
			bbits = 255;
		}
		else if (color[0] == 'G'&&color[1] == 'R'&&color[2] == 'E'&&color[3] == 'Y')
		{
			rbits = 128;
			gbits = 128;
			bbits = 128;
		}
		else if (color[0] == 'G'&&color[1] == 'R'&&color[2] == 'E'&&color[3] == 'E'&&color[4] == 'N')
		{
			rbits = 0;
			gbits = 255;
			bbits = 0;
		}
		else if (color[0] == 'P'&&color[1] == 'U'&&color[2] == 'R'&&color[3] == 'P'&&color[4] == 'L'&&color[5] == 'E')
		{
			rbits = 128;
			gbits = 0;
			bbits = 128;
		}
		else if (color[0] == 'B'&&color[1] == 'R'&&color[2] == 'O'&&color[3] == 'W'&&color[4] == 'N')
		{
			rbits = 92;
			gbits = 64;
			bbits = 51;
		}
}
void drawcircle(float circles[][8], int &index, int rbits, int gbits, int bbits, float thickness, int radius,int penup)
{
	float x, y;
	sf::Transform transform = triangle.getTransform();
	x = transform.transformPoint(triangle.getPoint(0)).x;
	y = transform.transformPoint(triangle.getPoint(0)).y;
	circles[index][0] = radius;
	circles[index][1] = thickness;
	circles[index][2] = x;
	circles[index][3] = y;
	if (penup == 0)
	{
		circles[index][4] = rbits;
		circles[index][5] = gbits;
		circles[index][6] = bbits;
	}
	else
	{
		circles[index][4] = 0;
		circles[index][5] = 0;
		circles[index][6] = 0;
	}
	circles[index][7] = penup;
	index++;

}
void pen(char str[], int &penup)
{
	if (compare2(str, "pu") == true)
		penup = 1;
	else if (compare2(str, "pd") == true)
		penup = 0;
}
void width(float newwidth, float&thickness)
{
	thickness = newwidth;
}
void clearscreen(int &lindex, int &cindex)
{
	lindex = 0;
	cindex = 0;
	triangle.setPoint(0, sf::Vector2f(720, 350));
	triangle.setPoint(1, sf::Vector2f(700, 375));
	triangle.setPoint(2, sf::Vector2f(740, 375));
	triangle.setFillColor(sf::Color::White);
	triangle.setPosition(triangle.getOrigin());
}
void repeat(int nooftimes, char prompts[][100], float operands[], int noofcommands, int &rbits, int &gbits, int &bbits, int &penup, 
	float lines[][8], int &lindex, float circles[][8], int &cindex, float &thickness, char colors[][100], char err[])
{
	int ci = 0,x;
	for (int i = 0; i < nooftimes; i++)
	{
		for (int j = 0; j < noofcommands; j++)
		{
			x = (int)operands[j];
			if (compall(prompts[j], "fd", 2, err, operands[j]) == true || compall(prompts[j], "forward", 7, err, operands[j]) == true)
				forward(lines, lindex, operands[j], penup, rbits, gbits, bbits, thickness);
			else if (compall(prompts[j], "bk", 2, err, operands[j]) == true || compall(prompts[j], "backward", 8, err, operands[j]) == true)
				backward(lines, lindex, operands[j], penup, rbits, gbits, bbits, thickness);
			else if (rotate1(prompts[j], "rt", err, x) == true)
				right(x);
			else if (rotate1(prompts[j], "lt", err, x) == true)
				left(x);
			else if (compare2(prompts[j], "pu") == true)
				pen(prompts[j], penup);
			else if (compare2(prompts[j], "pd") == true)
				pen(prompts[j], penup);
			else if (compall(prompts[j], "circle", 6, err, operands[j]) == true)
				drawcircle(circles, cindex, rbits, gbits, bbits, thickness, operands[j],penup);
			else if (compall(prompts[j], "width", 5, err, operands[j]) == true)
				width(operands[j], thickness);
			else if (compare2(prompts[j], "cs") == true)
				clearscreen(lindex, cindex);
			else if (color(prompts[j], colors[ci], err) == true)
			{
				selectcolor(colors[ci], rbits, gbits, bbits);
				ci++;
			}

		}
	}
}
void save(char twod[][100], char filename[], int numrows)
{
	ofstream fout(filename);
	int j = 0;
	if (numrows > 100)
		j = numrows - 100;
	for (int i = j; i < numrows; i++)
	{
		fout << twod[i] << endl;
	}
	fout.close();
}
void load(char commandarr[][100], char arr[], int &commandarrind, float &steps, int &angle, char error[], float &lineThickness, float &thick,
	float &radius, int&penup, int &noofrepeatcommands,int &repeattimes,float repeatoperands[],char repcommands[][100],char repfunctions[][100],char repcolors[][100],
	char filename[],float lines[][8],int &lindex, float circles[][8],int &cindex, int &redbits, int &greenbits, int &bluebits,char color1[])
{
	ifstream fin(arr);
	char str[100][100];
	string st;
	int length = 0;
	int count = 0;
	for (count = 0; count < 100; count++)
	{
		length = 0;
		fin.getline( str[count],100);
		while (str[count][length] != 0)
			length++;
		if (compare2(str[count], "pu") || compare2(str[count], "pd"))
		{
			pen(str[count], penup);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (compall(str[count], "fd", 2, error, steps) == true || compall(str[count], "forward", 7, error, steps) == true)
		{
			forward(lines, lindex, steps, penup, redbits, greenbits, bluebits, lineThickness);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (compall(str[count], "bk", 2, error, steps) == true || compall(str[count], "backward", 8, error, steps) == true)
		{
			backward(lines, lindex, steps, penup, redbits, greenbits, bluebits, lineThickness);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (compall(str[count], "width", 5, error, thick) == true)
		{
			width(thick, lineThickness);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (compall(str[count], "circle", 6, error, radius) == true)
		{
			drawcircle(circles, cindex, redbits, greenbits, bluebits, lineThickness, radius, penup);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (compare2(str[count], "cs") == true)
		{
			clearscreen(lindex, cindex);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if ((repeatval(str[count], repeattimes, repeatoperands, repcommands, repcolors, repfunctions, error, noofrepeatcommands) == true))
		{
			repeat(repeattimes, repcommands, repeatoperands, noofrepeatcommands, redbits, greenbits, bluebits, penup, lines, lindex, circles, cindex, lineThickness, repcolors, error);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (rotate1(str[count], "rt", error, angle) == true)
		{
			right(angle);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (rotate1(str[count], "lt", error, angle) == true)
		{
			left(angle);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (color(str[count], color1, error) == true)
		{
			selectcolor(color1, redbits, greenbits, bluebits);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}
		else if (filework(str[count], "save", filename, error) == true)
		{
			save(commandarr, filename, commandarrind);
			for (int i = 0; i <= length; i++)
			{
				commandarr[commandarrind][i] = str[count][i];
			}
			commandarrind++;
		}

	}
	fin.close();
}
int main()
{
	sf::RenderWindow window({ 1500, 750 }, "TABLET INTERFACE");
	sf::Event event;
	sf::Font font1;

	if (!font1.loadFromFile("C:\\Windows\\Fonts\\Arial.ttf"))
	{
		// error...
	}
	triangle.setPointCount(3);
	triangle.setPoint(0, sf::Vector2f(720, 350));
	triangle.setPoint(1, sf::Vector2f(700, 375));
	triangle.setPoint(2, sf::Vector2f(740, 375));
	triangle.setFillColor(sf::Color::White);
	sf::Vector2f position = triangle.getOrigin();
	float lines[100][8]; int  lindex = 0;
	int redbits = 255, greenbits = 0, bluebits = 0;
	float circles[100][8]; int cindex = 0;
	char color1[50];
	sf::Transform transform = triangle.getTransform();
	sf::Text itext("hello", font1);
	itext.setFont(font1);
	itext.setPosition(sf::Vector2f(0.0, 700.0));
	itext.setCharacterSize(30);
	itext.setStyle(sf::Text::Regular);
	itext.setFillColor(sf::Color::Red);
	sf::Text otext("hello", font1);
	otext.setFont(font1);
	otext.setPosition(sf::Vector2f(1200.0, 600.0));
	otext.setCharacterSize(15);
	otext.setStyle(sf::Text::Regular);
	otext.setFillColor(sf::Color::Red);
	sf::Text etext("hello", font1);
	etext.setFont(font1);
	etext.setPosition(sf::Vector2f(0.0, 575.0));
	etext.setCharacterSize(30);
	etext.setStyle(sf::Text::Regular);
	etext.setFillColor(sf::Color::Red);
	float steps = 0.0;
	int angle = 0;
	char str[100], commandarr[1000][100], error[100];
	int count = 0, commandarrind = 0, penup = 0;
	float lineThickness = 1.0;
	float thick = 0.0, radius = 0.0;
	int noofrepeatcommands = 0, repeattimes = 0;
	float repeatoperands[100];
	char repcommands[100][100], repfunctions[100][100], repcolors[100][100],filename[50];
	for (int i = 0; i < 100; i++)
	{
		repeatoperands[i] = 0.0;
	}
	std::string inputtext;
	std::string outputtext;
	std::string errortext;
	outputtext = "";
	errortext = "";
	for (int i = 0; i < 100; i++)
		commandarr[i][0] = 0;
	str[0] = 0;


	while (window.isOpen())
	{
		while (window.pollEvent(event))
		{
			if (event.type == sf::Event::Closed)
			{
				window.close();

			}
			else if (event.type == sf::Event::KeyPressed)
			{
				
				if (event.key.code != sf::Keyboard::Enter)
				{

					if (event.key.code >= 0 && event.key.code <= 25)

					{
						if (sf::Keyboard::isKeyPressed(sf::Keyboard::LShift) || sf::Keyboard::isKeyPressed(sf::Keyboard::RShift))
						{
							str[count++] = 'A' + event.key.code;
							str[count] = 0;
						}
						else
						{
							str[count++] = 'a' + event.key.code;
							str[count] = 0;
						}
					}

					if (event.key.code >= 26 && event.key.code <= 35)
					{
						str[count++] = event.key.code + 22;
						str[count] = 0;
					}
					if (event.key.code == 57)
					{
						str[count++] = 32;
						str[count] = 0;
					}
					if (event.key.code == 59 && count > 0)
					{
						count--;
						str[count] = 0;
					}
					if (event.key.code == sf::Keyboard::LBracket)
					{
						str[count++] = 91;
						str[count] = 0;
					}
					if (event.key.code == sf::Keyboard::RBracket)
					{
						str[count++] = 93;
						str[count] = 0;
					}

					if (event.key.code == sf::Keyboard::Comma)
					{
						str[count++] = 44;
						str[count] = 0;
					}
					if (event.key.code == 51)
					{
						str[count++] = 34;
						str[count] = 0;
					}
					if (event.key.code == 50)
					{
						str[count++] = 46;
						str[count] = 0;
					}
					inputtext= str;
					itext.setString(inputtext);
					window.draw(itext);
					etext.setString(errortext);
					window.draw(etext);
				}
				if (event.key.code == sf::Keyboard::Enter)
				{
					
					if (compare2(str, "pu") || compare2(str, "pd"))
					{
						pen(str, penup);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (compall(str, "fd", 2, error, steps) == true || compall(str, "forward", 7, error, steps) == true)
					{
						forward(lines, lindex, steps, penup, redbits, greenbits, bluebits, lineThickness);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (compall(str, "bk", 2, error, steps) == true || compall(str, "backward", 8, error, steps) == true)
					{
						backward(lines, lindex, steps, penup, redbits, greenbits, bluebits, lineThickness);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (compall(str, "width", 5, error, thick) == true)
					{
						width(thick, lineThickness);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (compall(str, "circle", 6, error, radius) == true)
					{
						drawcircle(circles, cindex, redbits, greenbits, bluebits, lineThickness, radius,penup);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (compare2(str, "cs") == true)
					{
						clearscreen(lindex, cindex);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if ((repeatval(str, repeattimes, repeatoperands, repcommands, repcolors, repfunctions, error, noofrepeatcommands) == true))
					{
						repeat(repeattimes, repcommands, repeatoperands, noofrepeatcommands, redbits, greenbits, bluebits, penup, lines, lindex, circles, cindex, lineThickness, repcolors, error);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (rotate1(str, "rt", error, angle) == true)
					{
						right(angle);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (rotate1(str, "lt", error, angle) == true)
					{
						left(angle);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);

					}
					else if (color(str, color1, error) == true)
					{
						selectcolor(color1, redbits, greenbits, bluebits);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (filework(str, "save",filename,error) == true)
					{
						for (int i = 0; i < strlen(filename); i++)
						{
							if (filename[i] == '"')
							{
								for (int j = i; j < strlen(filename); j++)
								{
									filename[j] = filename[j + 1];
								}
							}
						}
						save(commandarr, filename, commandarrind);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else if (filework(str, "load", filename,error) == true)
					{		
						for (int i = 0; i < strlen(filename); i++)
						{
							if (filename[i] == '"')
							{
								for (int j = i; j < strlen(filename); j++)
								{
									filename[j] = filename[j + 1];
								}
							}
						}
						load(commandarr, filename, commandarrind,steps,angle,error,lineThickness,thick,radius,penup,noofrepeatcommands,repeattimes,repeatoperands,
							repcommands,repfunctions,repcolors,filename,lines,lindex,circles,cindex,redbits,greenbits,bluebits,color1);
						for (int i = 0; i <= count; i++)
						{
							commandarr[commandarrind][i] = str[i];
						}
						commandarrind++;
						inputtext = str;
						count = 0;
						str[0] = 0;
						outputtext = "";
						errortext = "";
						itext.setString("");
						otext.setString("");
						etext.setString("");
						window.draw(etext);
						window.draw(itext);
					}
					else
					{
						if (compare1(error, "Incomplete command", 18) == false)
						{
							error[0] = 'I'; error[1] = 'n'; error[2] = 'v'; error[3] = 'a';
							error[4] = 'l'; error[5] = 'i'; error[6] = 'd'; error[7] = ' ';
							error[8] = 'c'; error[9] = 'o'; error[10] = 'm'; error[11] = 'm';
							error[12] = 'a'; error[13] = 'n'; error[14] = 'd'; error[15] = 0;
						}

						errortext = error;
						error[0] = 0;
						inputtext = str;
						str[0] = 0;
						count = 0;
						itext.setString("");
						etext.setString("");
					}
					
				}
			}

		}
		window.clear();
		for (int i = 0; i < lindex; i++)
		{
			rectangle.setSize(sf::Vector2f(lines[i][2], lines[i][3]));
			rectangle.setPosition(lines[i][0], lines[i][1]);
			if (lines[i][7] == 0)
			{
				rectangle.setFillColor(sf::Color(lines[i][4], lines[i][5], lines[i][6]));
				rectangle.setOutlineColor(sf::Color(lines[i][4], lines[i][5], lines[i][6]));
			}
			else if (lines[i][7]==1)
			{
				rectangle.setFillColor(sf::Color::Transparent);
				rectangle.setOutlineColor(sf::Color::Transparent);
			}

			window.draw(rectangle);
		}
		for (int j = 0; j < cindex; j++)
		{
			circle.setRadius(circles[j][0]);
			circle.setOutlineThickness(circles[j][1]);
			circle.setPosition(circles[j][2] - circles[j][0], circles[j][3] - circles[j][0]);
			if (circles[j][7] == 0)
				circle.setOutlineColor(sf::Color(circles[j][4], circles[j][5], circles[j][6]));
			else if (circles[j][7]==1)
				circle.setOutlineColor(sf::Color::Transparent);
			circle.setFillColor(sf::Color::Transparent);
			window.draw(circle);
		}
		outputtext = "Command History:\n";
		for (int j = commandarrind - 5; j < commandarrind; j++)
		{

			for (int k = 0; k < 100 && commandarr[j][k] != 0; k++)
			{
				if (j >= 0)
					outputtext += commandarr[j][k];

			}
			if (j >= 0)
				outputtext += "\n";
			otext.setString(outputtext);
			window.draw(otext);
			otext.setString("");
		}
		itext.setString(inputtext);
		window.draw(itext);
		etext.setString(errortext);
		window.draw(etext);
		window.draw(triangle);
		window.display();
	}
}

