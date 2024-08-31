##########################################################
#                                                        #
#           Créateur : Marijan Stajic                    #
#           Dernière modification : 22.05.2023           #
#           Date de création : 24.03.2023                #
#                                                        #
#           Mini jeux développé pour mes cours           #
#           à l'EPSIC.                                   #
#                                                        #
##########################################################

# Importation des modules
import pygame    
import random
import csv
from os import path

# Initialisation du module pygame
pygame.init()

# Initialisation du son
pygame.mixer.init()

# Chemin du dossier ressources
ressources_path = path.join(path.dirname(__file__), "Ressources")

# Nom de la fenêtre
pygame.display.set_caption("Palmac")

# Fonction concernant le taux de rafraîchissement
clock = pygame.time.Clock()

# Temps réel du jeux
time_disappear = pygame.time.get_ticks()

# Dimension de la fenêtre
screen = pygame.display.set_mode((400,600))

# Gestion des touches du personnage principal (clavier)
class Key():

    def __init__(self):
        self.pressed = {}

key = Key()

# Gestion des touches du personnage principal (souris)
class Mouse():

    def __init__(self):
        self.pressed = {"left": False, "right": False}

mouse = Mouse()

# Player
# Initialisation de variable velocity de base
player_velocity = 5

# Création de Player
class Player(pygame.sprite.Sprite):

    def __init__(self):
        super().__init__()
        self.image = pygame.image.load(path.join(ressources_path,'Images/palma_one.png'))
        self.image_life = pygame.image.load(path.join(ressources_path,'Images/palma_life.png'))
        self.image_cry = pygame.image.load(path.join(ressources_path,'Images/palma_cry.png'))
        self.rect = self.image.get_rect()
        self.radius = 24
        self.rect.x = 155
        self.rect.y = 490
        self.velocity = player_velocity

    def move_right(self):
        self.rect.x += self.velocity

    def move_left(self):
        self.rect.x -= self.velocity

player = Player()

# Ennemis
# Initialisation de variable de base
enemie_velocity = 3

# Création des Ennemis
class Enemy(pygame.sprite.Sprite):

    def __init__(self):
        super().__init__()
        self.image = pygame.image.load(path.join(ressources_path,'Images/enemy.png'))
        self.rect = self.image.get_rect()
        self.radius = 35
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(0, 325)
        self.rect.y = -73
        self.velocity = enemie_velocity
    
    def update(self):
        self.rect.y += self.velocity

all_enemies = pygame.sprite.Group()

# Gestion de l'apparition d'ennemies
last_enemy_spawn = 0
time_spawn_enemy = 900

# Bonus
# Initialisation de variable de base
bonus_velocity = 3

# Création des Bonus
class Bonus(pygame.sprite.Sprite):

    def __init__(self):
        super().__init__()
        self.image = pygame.image.load(path.join(ressources_path,'Images/bonus.png'))
        self.rect = self.image.get_rect()
        self.radius = 40
        #pygame.draw.circle(self.image, green, self.rect.center, self.radius)
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(0, 325)
        self.rect.y = -73
        self.velocity = bonus_velocity
    
    def update(self):
        self.rect.y += self.velocity

all_bonus = pygame.sprite.Group()

# Gestion de l'apparition des bonus
last_bonus_spawn = 0
time_spawn_bonus = 900

# Gestion de la colision entre l'ennemie et le bonus
collide_enemy_bonus = False

# Activation des dégâts
hits_enemies_enable = True

# Activation des bonus
hits_bonus_enable = True

# Points de vie
life_player = 3
life = pygame.image.load(path.join(ressources_path,'Images/life.png'))

# Score du joueur
score_value = 0
last_score_level_value = 0
last_score_speed_value = 0
score_font = pygame.font.Font(path.join(ressources_path,'Polices/Minecraft.ttf'), 20)
score_15 = pygame.image.load(path.join(ressources_path,'Images/+15_score.png'))
score_15_display = False

# Niveau
level_value = 1
level_font = pygame.font.Font(path.join(ressources_path,'Polices/Minecraft.ttf'), 20)

# Tableau des scores
scoreboard = pygame.image.load(path.join(ressources_path,'Images/scoreboard.png'))
scoreboard_font = pygame.font.Font(path.join(ressources_path,'Polices/Minecraft.ttf'), 28)

# Tableau des informations
board_information = 1
board_information_1 = pygame.image.load(path.join(ressources_path,'Images/information_1.png'))
board_information_2 = pygame.image.load(path.join(ressources_path,'Images/information_2.png'))
player_name_board_information_1_font = pygame.font.Font(path.join(ressources_path,'Polices/Minecraft.ttf'), 30)

# Nom du joueur
player_name = "Palma"
font_change_name = pygame.font.Font(path.join(ressources_path,'Polices/Minecraft.ttf'), 24)
menu_change_name = pygame.image.load(path.join(ressources_path,'Images/menu_change_name.png'))
input_user_change_name = "Modifier"
active_change_name = False

# Logo du jeux
logo_palmac = pygame.image.load(path.join(ressources_path,'Images/logo_palmac.png'))

# Background
background = pygame.image.load(path.join(ressources_path,'Images/background.png'))
background_game = pygame.image.load(path.join(ressources_path,'Images/background_game.png'))
y_background_game = 0
background_break = pygame.image.load(path.join(ressources_path,'Images/background_break.png'))
background_present = pygame.image.load(path.join(ressources_path,'Images/background_present.png'))
background_scoreboard_game = pygame.image.load(path.join(ressources_path,'Images/background_scoreboard_game.png'))

# Bouton (Play)
button_play = pygame.image.load(path.join(ressources_path,'Images/Button/button_play.png'))
button_play_rect = button_play.get_rect()
button_play_rect.x += 90
button_play_rect.y += 250
button_play_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_play_hover.png'))
button_play_hover_rect = button_play_hover.get_rect()
button_play_hover_rect.x += 90
button_play_hover_rect.y += 250

# Bouton (Information)
button_information = pygame.image.load(path.join(ressources_path,'Images/Button/button_information.png'))
button_information_rect = button_information.get_rect()
button_information_rect.x += 90
button_information_rect.y += 350
button_information_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_information_hover.png'))
button_information_hover_rect = button_information_hover.get_rect()
button_information_hover_rect.x += 90
button_information_hover_rect.y += 350

# Bouton (Quitter)
button_quit = pygame.image.load(path.join(ressources_path,'Images/Button/button_quit.png'))
button_quit_rect = button_quit.get_rect()
button_quit_rect.x += 90
button_quit_rect.y += 450
button_quit_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_quit_hover.png'))
button_quit_hover_rect = button_quit_hover.get_rect()
button_quit_hover_rect.x += 90
button_quit_hover_rect.y += 450

# Bouton (Suivant)
button_next = pygame.image.load(path.join(ressources_path,'Images/Button/button_next.png'))
button_next_rect = button_next.get_rect()
button_next_rect.x += 90
button_next_rect.y += 510
button_next_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_next_hover.png'))
button_next_hover_rect = button_next_hover.get_rect()
button_next_hover_rect.x += 90
button_next_hover_rect.y += 510

# Bouton (Changer de nom)
button_change_name = pygame.image.load(path.join(ressources_path,'Images/Button/button_change_player_name.png'))
button_change_name_rect = button_change_name.get_rect()
button_change_name_rect.x += 90
button_change_name_rect.y += 145
button_change_name_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_change_player_name_hover.png'))
button_change_name_hover_rect = button_change_name_hover.get_rect()
button_change_name_hover_rect.x += 90
button_change_name_hover_rect.y += 145
mouse_clicked_change_name = False

# Bouton (Relancer)
button_restart = pygame.image.load(path.join(ressources_path,'Images/Button/button_restart.png'))
button_restart_rect = button_restart.get_rect()
button_restart_rect.x += 90
button_restart_rect.y += 295
button_restart_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_restart_hover.png')) 
button_restart_hover_rect = button_restart_hover.get_rect()
button_restart_hover_rect.x += 90
button_restart_hover_rect.y += 295

# Bouton (Exporter)
button_export = pygame.image.load(path.join(ressources_path,'Images/Button/button_export.png'))
button_export_rect = button_export.get_rect()
button_export_rect.x += 90
button_export_rect.y += 395
button_export_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_export_hover.png'))
button_export_hover_rect = button_export_hover.get_rect()
button_export_hover_rect.x += 90
button_export_hover_rect.y += 395

# Export message
export_ok = pygame.image.load(path.join(ressources_path,'Images/export_ok.png'))
export = False

# Bouton (Retourner)
button_return = pygame.image.load(path.join(ressources_path,'Images/Button/button_return.png'))
button_return_rect = button_return.get_rect()
button_return_rect.x += 90
button_return_rect.y += 495
button_return_hover = pygame.image.load(path.join(ressources_path,'Images/Button/button_return_hover.png')) 
button_return_hover_rect = button_return_hover.get_rect()
button_return_hover_rect.x += 90
button_return_hover_rect.y += 495
button_return_active = False

# Bouton (Son)
button_sound_on = pygame.image.load(path.join(ressources_path,'Images/Button/button_sound_on.png'))
button_sound_on_rect = button_sound_on.get_rect()
button_sound_on_rect.x += 355
button_sound_on_rect.y += 5
button_sound_off = pygame.image.load(path.join(ressources_path,'Images/Button/button_sound_off.png'))
button_sound_off_rect = button_sound_off.get_rect()
button_sound_off_rect.x += 355
button_sound_off_rect.y += 5
sound_pressed = True
mouse_clicked_sound = False

# Fonction du bouton (Son)
def sound_button():
        global sound_pressed, mouse_clicked_sound, menu_sound

        if sound_pressed:
            screen.blit(button_sound_on, button_sound_on_rect)
            menu_sound.set_volume(0.1)
        else:
            screen.blit(button_sound_off, button_sound_off_rect)
            menu_sound.set_volume(0)
            
        if event.type == pygame.MOUSEBUTTONDOWN:
            if mouse.pressed["left"] == True:
                if button_sound_on_rect.collidepoint(event.pos):
                    mouse_clicked_sound = True 
                
        if event.type == pygame.MOUSEBUTTONUP:
            if mouse.pressed["left"] == False:
                if button_sound_on_rect.collidepoint(event.pos) and mouse_clicked_sound:
                    button_click_sound.play()
                    sound_pressed = not sound_pressed
                    mouse_clicked_sound = False 

# Son des effets
button_click_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/button_click_sound.mp3'))
score_15_effect_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/score_15_effect_sound.mp3'))
damage_effect_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/damage_effect_sound.mp3'))
regeneration_effect_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/regeneration_effect_sound.mp3'))
level_up_effect_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/level_up_effect_sound.mp3'))
menu_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/menu_sound.mp3'))
game_sound = pygame.mixer.Sound(path.join(ressources_path,'Sound/game_sound.mp3'))

# Régler le volume de tous les sons à 50%
for sound in [button_click_sound, score_15_effect_sound, damage_effect_sound, regeneration_effect_sound, level_up_effect_sound]:
    sound.set_volume(0.3)

# Fonction sur le reset de la partie
def reset_value_game():
    global player_velocity, enemie_velocity, bonus_velocity, last_bonus_spawn, time_spawn_bonus, time_spawn_enemy, last_enemy_spawn, level_value, score_value, life_player, hits_enemies_enable, last_score_level_value, last_score_speed_value

    player_velocity = 5
    enemie_velocity = 3
    bonus_velocity = 3
    time_spawn_bonus = 900
    last_bonus_spawn = 0
    time_spawn_enemy = 900
    last_enemy_spawn = 0
    life_player = 3
    level_value = 1
    score_value = 0
    last_score_level_value = 0
    last_score_speed_value = 0
    for bonus in all_bonus:
        all_bonus.remove(bonus)
    for enemy in all_enemies:
        all_enemies.remove(enemy)
    player.rect.x = random.randrange(0, 325)
    hits_enemies_enable = True

# Boucle principal du jeu
screen_running = True
screen_running_present = True
screen_running_menu = False
screen_running_game = False    
screen_running_break = False
screen_running_information = False

while screen_running :  

    # Intéraction de l'utilisateur
    for event in pygame.event.get():
        # Si il décide de fermer le jeu - Donc stop la fenêtre
        if event.type == pygame.QUIT:    
            screen_running = False
        # Si il enfonce la touche
        elif event.type == pygame.KEYDOWN:
            key.pressed[event.key] = True
        # Si il relâche la touche
        elif event.type == pygame.KEYUP:
            key.pressed[event.key] = False
        # Si il enfonce le clique
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 1:
                mouse.pressed["left"] = True
            elif event.button == 2:
                mouse.pressed["right"] = True
        # Si il relâche le clique
        elif event.type == pygame.MOUSEBUTTONUP:
            if event.button == 1:
                mouse.pressed["left"] = False
            elif event.button == 2:
                mouse.pressed["right"] = False

    if screen_running_present:

        # Rafraîchissement de l'écran et du fond présentation
        pygame.display.flip()
        screen.blit(background_present, (0,0))

        # Rendre la souris invisible
        pygame.mouse.set_visible(False)

        time_since_disappear = pygame.time.get_ticks() - time_disappear
        if time_since_disappear > 3000:
            screen_running_menu = True
            screen_running_present = False
            # Musique de fond
            menu_sound.play(-1)

    if screen_running_menu:
        
        # Rafraîchissement de l'écran, du fond et des images
        pygame.display.flip()
        screen.blit(background, (0,0))

        # Rendre la souris visible
        pygame.mouse.set_visible(True)

        # Affichage du logo
        screen.blit(logo_palmac, (20, 65))
        
        # Affichage du bouton (Jouer)
        screen.blit(button_play, button_play_rect)

        if event.type == pygame.MOUSEMOTION:
            if button_play_rect.collidepoint(event.pos):
                screen.blit(button_play_hover, button_play_hover_rect)
        elif mouse.pressed["left"] == True:
            if button_play_rect.collidepoint(event.pos):
                button_click_sound.play()
                screen_running_game = True
                screen_running_menu = False
                game_sound.play(-1)
                game_sound.set_volume(0.1)
                menu_sound.stop()
        elif event.type == pygame.MOUSEBUTTONUP:
                mouse.pressed["left"] = False

        # Affichage du bouton (Information)
        screen.blit(button_information, button_information_rect)

        if event.type == pygame.MOUSEMOTION:
            if button_information_rect.collidepoint(event.pos):
                screen.blit(button_information_hover, button_information_hover_rect)
        elif mouse.pressed["left"] == True:
            if button_information_rect.collidepoint(event.pos):
                button_click_sound.play()
                screen_running_information = True
                screen_running_menu = False
        elif event.type == pygame.MOUSEBUTTONUP:
                mouse.pressed["left"] = False

        # Affichage du bouton (Quitter)
        screen.blit(button_quit, button_quit_rect)

        if event.type == pygame.MOUSEMOTION:
            if button_quit_rect.collidepoint(event.pos):
                screen.blit(button_quit_hover, button_quit_hover_rect)
        elif mouse.pressed["left"] == True:
            if button_quit_rect.collidepoint(event.pos):
                button_click_sound.play()
                screen_running = False
        elif event.type == pygame.MOUSEBUTTONUP:
                mouse.pressed["left"] = False
        
        # Affichage du bouton (Son)
        sound_button()

    if screen_running_game:

        # Si l'utilisateur appuie sur P pour mettre en pause la partie
        if key.pressed.get(pygame.K_p):
            screen_running_break = True    

        # Rendre la souris invisible
        pygame.mouse.set_visible(False)

        if not screen_running_break: 
            
            # Rafraîchissement de l'écran
            pygame.display.flip()

            # Background animé
            y_background_game += 1
            if y_background_game < 600:
                screen.blit(background_game, (0,y_background_game))
                screen.blit(background_game, (0,y_background_game - 600))
            else:
                y_background_game = 0
                screen.blit(background_game, (0,y_background_game))

            # Background au niveau du score, niveau et des vies
            screen.blit(background_scoreboard_game, (0,0))
            
            # Musique
            menu_sound.set_volume(0)

            # Images par seconde
            clock.tick(60)

            # Affichage du Player (normal, triste, heureux)
            if not hits_enemies_enable or not hits_bonus_enable:
                if not hits_bonus_enable:
                    screen.blit(player.image_life, player.rect)
                if not hits_enemies_enable:
                    hits_bonus_enable = True
                    screen.blit(player.image_cry, player.rect)
                    
                    # Clignotement du Player 
                    time_since_disappear = pygame.time.get_ticks() - time_disappear
                    if 500 <= time_since_disappear < 1000 or 1500 <= time_since_disappear < 2000 or 2500 <= time_since_disappear < 3000:
                        player.image_cry.set_alpha(50)
                    else:
                        player.image_cry.set_alpha(300)

            else:
                screen.blit(player.image, player.rect)

            # Affichage des ennemies et des bonus
            all_enemies.draw(screen)
            all_bonus.draw(screen)

            # Mise à jour des positions
            all_enemies.update()
            all_bonus.update()

            # Affichage du score
            score_display = score_font.render("Score: " + str(score_value), True, (255, 255, 255))
            screen.blit(score_display, (10, 10))
            
            # Affichage du +15
            if score_15_display == True:
                if score_value < 100:
                    score_15_display_y = 108
                else:
                    score_15_display_y = 116

                screen.blit(score_15, (score_15_display_y, 10))
                time_since_disappear = pygame.time.get_ticks() - time_disappear
                if time_since_disappear > 3000:
                    score_15_display = False

            # Affichage des niveaux
            level_display = level_font.render("Niveau: " + str(level_value), True, (255, 255, 255))
            screen.blit(level_display, (10, 40))

            # Gestion des points de vie
            if life_player == 3:
                screen.blit(life, (345, 5))
                screen.blit(life, (295, 5))
                screen.blit(life, (245, 5))

            if life_player == 2:
                screen.blit(life, (345, 5))
                screen.blit(life, (295, 5))

            if life_player == 1:
                screen.blit(life, (345, 5))
    
            if life_player == 0:
                screen_running_game = False
                # Démarrer la musique du menu
                menu_sound.play()
                menu_sound.set_volume(0.1)

            # Si l'utilisateur appuie sur la fleche de droite pour ce déplacer
            if key.pressed.get(pygame.K_RIGHT) and player.rect.x < 345:
                player.move_right()

            # Si l'utilisateur appuie sur la fleche de gauche pour ce déplacer
            if key.pressed.get(pygame.K_LEFT) and player.rect.x > -1:
                player.move_left()

            # Gestion des collisions (ennemies) - Lorsque les collisions sont activées
            if hits_enemies_enable ==  True:
                hits_enemies = pygame.sprite.spritecollide(player, all_enemies, False, pygame.sprite.collide_circle)
        
                # Si le personnage principal est touché par les ennemies
                if hits_enemies:
                    damage_effect_sound.play()
                    life_player -= 1
                    hits_enemies_enable = False
                    time_disappear = pygame.time.get_ticks()
                    for enemy in all_enemies:
                        if pygame.sprite.collide_circle(enemy, player):
                            all_enemies.remove(enemy)

            # Lorsque les collisions sont désactivée à cause des dégâts
            if hits_enemies_enable == False:
                time_since_disappear = pygame.time.get_ticks() - time_disappear
                if time_since_disappear > 3000:
                    hits_enemies_enable = True

            # Suppression de l'ennemie
            for enemy in all_enemies:
                if enemy.rect.y > 600:
                    all_enemies.remove(enemy)
                    # Augementation du score si les dégâts sont activée
                    if hits_enemies_enable == False:
                        None
                    else:
                        score_value += 1
                        if score_value > 1:
                            last_score_level_value += 1
                            last_score_speed_value += 1
                            # Augmentation du niveau
                            if last_score_level_value > 24:
                                level_up_effect_sound.play()
                                save_last_score_level_value = last_score_level_value - 25
                                last_score_level_value = 0
                                level_value +=1
                                last_score_level_value = save_last_score_level_value
                            # Augementation de la vitesse
                            if last_score_speed_value > 15:
                                save_last_score_speed_value = last_score_speed_value - 15
                                last_score_speed_value = 0 
                                enemie_velocity += 0.5
                                bonus_velocity += 0.5
                                player_velocity += 0.3
                                if time_spawn_enemy > 210:
                                    time_spawn_enemy -= 30
                                if time_spawn_bonus > 440:    
                                    time_spawn_bonus -= 20
                                save_last_score_speed_value = save_last_score_speed_value

            # Calcul pour la réapparition des ennemies
            current_time = pygame.time.get_ticks()
            time_since_last_enemy_spawn = current_time - last_enemy_spawn
            if time_since_last_enemy_spawn > time_spawn_enemy:
                all_enemies.add(Enemy())
                last_enemy_spawn = current_time

            # Gestion des collisions (bonus) - Lorsque les collisions sont activées
            for bonus in all_bonus:
                if hits_enemies_enable == False:
                    None
                # Bonus désactivé si les dégâts sont désactivée
                elif hits_bonus_enable ==  True:
                    hits_bonus = pygame.sprite.spritecollide(player, all_bonus, False, pygame.sprite.collide_circle)

                    # Si le personnage principal est touché par les bonus
                    if hits_bonus:
                        # Si il a déjà 3 vie, augementation +15 du score
                        if life_player == 3:
                            score_15_effect_sound.play()
                            score_15_display = True
                            score_value += 15
                            last_score_level_value += 15
                            last_score_speed_value += 15
                            hits_bonus_enable = False
                            screen.blit(player.image_life, player.rect)
                            time_disappear = pygame.time.get_ticks()
                            for bonus in all_bonus:
                                if pygame.sprite.collide_circle(bonus, player):
                                    all_bonus.remove(bonus)
                        # Si il lui manque une vie, augementation des vies
                        else:
                            regeneration_effect_sound.play()
                            life_player += 1
                            hits_bonus_enable = False
                            screen.blit(player.image_life, player.rect)
                            time_disappear = pygame.time.get_ticks()
                            for bonus in all_bonus:
                                if pygame.sprite.collide_circle(bonus, player):
                                    all_bonus.remove(bonus)

            # Désactivation des dégâts pendant 5 secondes
            if hits_bonus_enable == False:
                time_since_disappear = pygame.time.get_ticks() - time_disappear
                if time_since_disappear > 3000:
                    hits_bonus_enable = True

            # Suppression du bonus
            for bonus in all_bonus:
                if bonus.rect.y > 600:
                    all_bonus.remove(bonus)

            # Calcul pour la réapparition des bonus
            current_time = pygame.time.get_ticks()
            time_since_last_bonus_spawn = current_time - last_bonus_spawn
            if time_since_last_bonus_spawn > random.randrange(20000, 30000):
                all_bonus.add(Bonus())
                last_bonus_spawn = current_time

            # Colision entre le bonus et l'ennemie et suppression de l'ennemie
            for bonus in all_bonus:
                collide_enemy_bonus = pygame.sprite.spritecollide(bonus, all_enemies, True)

                if collide_enemy_bonus:
                    for enemy in all_enemies:
                        enemy.remove()        
        else:
            # Rafraîchissement de l'écran et affichage du message "Pause"
            pygame.display.flip()
            screen.blit(background_break, (0, 0))

            # Reduire le son de la musique
            game_sound.set_volume(0)

            # Si l'utilisateur appuie sur C pour continuer la partie
            if key.pressed.get(pygame.K_c):
                screen_running_break = False
                game_sound.set_volume(0.1)
    else:
        # Si les menus sont actifs, ne pas passer sur l'écran de résumé
        if screen_running_menu or screen_running_present or screen_running_information:
            None
        else:
            # Stopper la musique du jeux
            game_sound.stop()

            # Rafraîchissement de l'écran et du fond
            pygame.display.flip()
            screen.blit(background, (0,0))

            # Rendre la souris visible
            pygame.mouse.set_visible(True)

            # Affichage du tableau des scores
            score_display = scoreboard_font.render("Score: " + str(score_value), True, (255, 255, 255))
            level_display = scoreboard_font.render("Niveau: " + str(level_value), True, (255, 255, 255))
            screen.blit(scoreboard, (5, 15))
            screen.blit(score_display, (40, 160))
            screen.blit(level_display, (40, 195))
            
            # Affichage du bouton (Rejouer)
            screen.blit(button_restart, button_restart_rect)

            if event.type == pygame.MOUSEMOTION:
                if button_restart_rect.collidepoint(event.pos):
                    screen.blit(button_restart_hover, button_restart_hover_rect)
            elif mouse.pressed["left"] == True:
                if button_restart_rect.collidepoint(event.pos):
                    button_click_sound.play()
                    screen_running_game = True
                    menu_sound.stop()
                    game_sound.play(-1)
                    game_sound.set_volume(0.1)
                    reset_value_game()
            elif event.type == pygame.MOUSEBUTTONUP:
                mouse.pressed["left"] = False

            # Affichage du bouton (Exporter)
            if not export:
                screen.blit(button_export, button_export_rect)

                if event.type == pygame.MOUSEMOTION:
                    if button_export_rect.collidepoint(event.pos):
                        screen.blit(button_export_hover, button_export_hover_rect)
                elif mouse.pressed["left"] == True:
                    if button_export_rect.collidepoint(event.pos):
                        button_click_sound.play()
                        Header = ["Joueur", "Score", "Niveau"]

                        Data = [
                            [player_name, score_value, level_value],
                        ]
                        
                        filename = (path.join(ressources_path,'Palmac_Export.csv'))

                        with open(filename, "w", newline="") as csvfile:
                            csvwriter = csv.writer(csvfile)
                            csvwriter.writerow(Header)
                            csvwriter.writerows(Data)
                            
                        export = True
                        time_disappear = pygame.time.get_ticks() 

                elif event.type == pygame.MOUSEBUTTONUP:
                    mouse.pressed["left"] = False

            # Message informant l'exportation accomplie
            if export:
                screen.blit(export_ok, (90, 395))
                time_since_disappear = pygame.time.get_ticks() - time_disappear
                if time_since_disappear > 2000:
                    export = False

            # Affichage du bouton (Retourner)
            screen.blit(button_return, button_return_rect)

            if event.type == pygame.MOUSEMOTION:
                if button_return_rect.collidepoint(event.pos):
                    screen.blit(button_return_hover, button_return_hover_rect)
            elif mouse.pressed["left"] == True:
                if button_return_rect.collidepoint(event.pos):
                    button_return_active = True
            elif event.type == pygame.MOUSEBUTTONUP:
                if button_return_rect.collidepoint(event.pos):
                    mouse.pressed["left"] = False
                    if button_return_active:
                        button_click_sound.play()
                        screen_running_menu = True
                        export = False
                        reset_value_game()  
                        button_return_active = False

            # Affichage du bouton (Son)
            sound_button()

    if screen_running_information:

        # Rafraîchissement de l'écran, du fond et des images
        pygame.display.flip()
        screen.blit(background, (0,0))
        
        # Rendre la souris visible
        pygame.mouse.set_visible(True)

        # Affichage du premier tableau d'information
        if board_information == 1:
            screen.blit(board_information_1, (8, 15))
            welcome_display = player_name_board_information_1_font.render("Bienvenue " + str(player_name), True, (255, 255, 255))
            screen.blit(welcome_display, (72, 110))

        # Affichage du deuxième tableau d'information
        elif board_information == 2:
            screen.blit(board_information_2, (8, 15))
        
        # Affichage du bouton (Changer nom)
        
        if board_information == 1:
            screen.blit(button_change_name, button_change_name_rect)
            
            if event.type == pygame.MOUSEMOTION:
                if button_change_name_rect.collidepoint(event.pos):
                    screen.blit(button_change_name_hover, button_change_name_hover_rect)
            elif event.type == pygame.MOUSEBUTTONDOWN:
                if mouse.pressed["left"] == True:
                    if button_change_name_rect.collidepoint(event.pos):
                        mouse_clicked_change_name = True

            elif event.type == pygame.MOUSEBUTTONUP:
                if mouse.pressed["left"] == False and mouse_clicked_change_name:
                    if button_change_name_rect.collidepoint(event.pos):
                        button_click_sound.play()            
                        active_change_name = True
                        mouse_clicked_change_name = False

        # Affichage du menu (Changer nom)
        if active_change_name == True:                
            texte_change_name = font_change_name.render(input_user_change_name, True, (255, 255, 255))
            screen.blit(menu_change_name, (90, 170))
            screen.blit(texte_change_name, (155, 200))
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RETURN:
                        button_click_sound.play() 
                        player_name = input_user_change_name[:8]
                        active_change_name = False
                    elif event.key == pygame.K_ESCAPE:
                        button_click_sound.play() 
                        active_change_name = False
                    elif event.key == pygame.K_BACKSPACE:
                        input_user_change_name = input_user_change_name[:-1]
                    else:
                        input_user_change_name += event.unicode
                        input_user_change_name = input_user_change_name[:8]

        # Affichage du bouton (Suivant)
        screen.blit(button_next, button_next_rect)

        if event.type == pygame.MOUSEMOTION:
            if button_next_rect.collidepoint(event.pos):
                screen.blit(button_next_hover, button_next_hover_rect)

        elif event.type == pygame.MOUSEBUTTONDOWN:
            if mouse.pressed["left"] == True:
                if button_next_rect.collidepoint(event.pos):
                    button_click_sound.play()
                    if board_information == 1:
                        board_information = 2
                        mouse.pressed["left"] = False
                    elif board_information == 2:
                        screen_running_information = False
                        screen_running_menu = True
                        board_information = 1
                        mouse.pressed["left"] = False
        
        # Affichage du bouton (Son)
        sound_button()
