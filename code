import math
import random
import time


state = 0


def delay(timer):
    time.sleep(timer)


class Enemy:
    def __init__(self, level):
        self.health = 15 + level * 30
        self.defense = level + math.floor(level * (1 + random.uniform(-1, 1)))
        self.attack = level + math.floor(level * (1 + random.uniform(-1, 1)))
        self.speed = level + math.floor(level * (1 + random.uniform(-1, 1)))

    def attack_function(self, enemy):
        return max(self.attack - math.floor(enemy.defense) / 2, 1)


class PC:
    def __init__(self):
        self.name = "John"
        self.total_health = 20
        self.health = 20
        self.defense = 1
        self.attack = 1
        self.speed = 1
        self.exp = 1
        self.level = 1

    def start_up(self):
        total = 4
        while total > 0:
            print("You have " + str(total) + " stat point(s) left. Where do you want to allocate it?")
            stat_choice = input("[A]ttack / [S]peed / [D]efence")
            while stat_choice not in ['a', 'A', 'S', 's', 'd', 'D']:
                stat_choice = input("Not valid input. [A]ttack / [S]peed / [D]efence")
            if stat_choice in ['a', 'A']:
                self.attack += 1
                total -= 1
            elif stat_choice in ['d', 'D']:
                self.defense += 1
                total -= 1
            elif stat_choice in ['S', 's']:
                self.speed += 1
                total -= 1
            delay(0.5)

    def level_up(self):
        print(self.name + " has levelled up!")
        self.total_health += 20 * math.floor(self.level/10 + 1)
        self.level += 1
        self.defense += 1 + math.floor(self.level/10)
        self.attack += 1 + math.floor(self.level/10)
        self.speed += 1 + math.floor(self.level/10)
        self.health = self.total_health
        self.exp = 0
        delay(0.5)

    def attack_function(self, enemy):
        return max(math.floor(self.attack - enemy.defense / 2), 1)

    def exp_gain(self, number):
        self.exp += number
        if self.exp >= 1000:
            self.level_up()

    def heal(self):
        return min(int(1 / 4 * self.total_health), self.total_health - self.health)

    def pc_combat(self, antag):
        action = input("[A]ttack or [H]eal")
        while action not in ['a', 'A', 'h', 'H']:
            delay(0.5)
            action = input("Input not valid. [A]ttack or [H]eal")
        if action in ['a', 'A']:
            damage = self.attack_function(antag)
            delay(0.5)
            print("Kapow! John did " + str(damage) + " point(s) of damage!")
            antag.health -= damage
        elif action in ['h', 'H']:
            self.health += self.heal()
            delay(0.5)
            print("John has restored his health to: " + str(self.health) + "/" + str(self.total_health))


class Combat:
    def __init__(self, protag):
        self.protag = protag
        self.end_flag = 0
        self.antag = Enemy(protag.level + int(random.randrange(0, 2)))

        def win_condition():
            if self.antag.health < 0:
                self.end_flag = 1
                print("Winner :)")
                delay(0.5)
                protag.exp_gain(100)

        def lose_condition():
            global state
            if self.protag.health < 0:
                self.end_flag = 1
                print("Loser :(")
                delay(0.5)
                state = - 1
                return 1

        def combat_round():
            if self.end_flag == 0:
                if protag.speed >= self.antag.speed:
                    protag.pc_combat(self.antag)
                    win_condition()
                    if self.end_flag == 0:
                        enemy_damage = self.antag.attack_function(protag)
                        delay(0.5)
                        print("Kapow! Enemy did " + str(enemy_damage) + " point(s) of damage! " + protag.name + " has " + str(
                            protag.health) + " health left.")
                        protag.health -= enemy_damage
                        lose_condition()
                else:
                    enemy_damage = self.antag.attack_function(protag)
                    delay(0.5)
                    print("Kapow! Enemy did " + str(enemy_damage) + " point(s) of damage! " + protag.name + " has " + str(
                        protag.health) + " health left.")
                    protag.health -= enemy_damage
                    lose_condition()
                    if self.end_flag == 0:
                        protag.pc_combat(self.antag)
                        win_condition()

        print("The enemy has presented themselves. Enemy HP is " + str(self.antag.health) + ".")
        while self.end_flag == 0:
            combat_round()


if __name__ == '__main__':
    player = PC()
    player.name = input("What is your name?")
    player.start_up()
    while not state:
        delay(0.5)
        menu_option = input("Welcome to the Arena. Would you like to [F]ight or [R]est?")
        while menu_option not in ['f', 'F', 'R', 'r', 'q', 'Q']:
            delay(0.5)
            menu_option = input("Not valid input. Would you like to [F]ight or [R]est?")
        if menu_option in ['f', 'F']:
            Combat(player)
        elif menu_option in ['R', 'r']:
            delay(0.5)
            print(player.name + ' feels well-rested, and has recovered his health.')
            player.health = player.total_health
        elif menu_option in ['Q', 'q']:
            delay(0.5)
            player.exp_gain(1000)
    if state == -1:
        delay(0.5)
        print("Game over :(")

# See PyCharm help at https://www.jetbrains.com/help/pycharm/
