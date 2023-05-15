# Media-Library-Management-System_project

    import datetime
    import os

# Define the media library game video's section data
    class LMS:
        """ This class is used to keep a record of game videos from a media library.
        It has a total of four modules: "Display Game", "Issue Game", "Return Game", "Add Game" """
        def __init__(self, list_of_games, medialibrary_name):
            self.list_of_games = list_of_games
            self.medialibrary_name = medialibrary_name
            self.games_dict = {}
            Id = 101

        # Open and read the file "List_of_games.txt"
        with open(self.list_of_games) as gm:
            content = gm.readlines()
        for line in content:
            self.games_dict[str(Id)] = {
                "games_title": line.replace("\n", ""),
                "lender_name": "",
                "issue_date": "",
                "Status": "Available"
            }
            Id += 1

    # Display the data
    def display_games(self):
        print("--------------------------List of Games------------------------------")
        print("Games ID", "\t", "Title")
        print("---------------------------------------------------------------------")
        for key, value in self.games_dict.items():
            print(key, "\t\t", value.get("games_title"), "- [", value.get("Status"),"]")

    def issue_games(self):
        # Check if the game exists
        games_id = input("Enter game ID: ")
        current_date = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        if games_id in self.games_dict.keys():
            if not self.games_dict[games_id]["Status"] == "Available":
                # Check if the game is assigned and the status is not available
                print(f"This game is already issued to {self.games_dict[games_id]['lender_name']} on {self.games_dict[games_id]['issue_date']}")
                return self.issue_games()

            # Check the username taking the current name the status into account
            elif self.games_dict[games_id]['Status'] == "Available":
                your_name = input("Enter your name: ")
                self.games_dict[games_id]['lender_name'] = your_name
                self.games_dict[games_id]['issue_date'] = current_date
                self.games_dict[games_id]['Status'] = "Already Issued"
                print("Game issued successfully!\n")
            else:
                print("Game ID not found!")

    def add_games(self):
        new_game = input("Enter game's title: ")

        # If the game's title is blank, return from the function
        if new_game == "":
            return self.add_games()

        # Restricted to the rule that the game title should not be over 55 characters
        elif len(new_game) > 55:
            print("Game's title length is too long! Title length should be 50 characters or less.")
            return self.add_games()

        else:
            with open(self.list_of_games, "a") as gm:
                gm.write(f"{new_game}\n")
                self.games_dict[str(int(max(self.games_dict)) + 1)] = {
                    'games_title': new_game,
                    'lender_name': '',
                    'issue_date': '',
                    'Status': 'Available'
                }
                print(f"The game '{new_game}' has been added successfully!")

    def return_games(self):
        games_id = input("Enter game ID: ")
        if games_id in self.games_dict.keys():
            if self.games_dict[games_id]["Status"] == "Available":
                print("This game is already available in the media library. Please check your game ID.")
                return self.return_games()
            else:
                self.games_dict[games_id]["lender_name"] = ""
                self.games_dict[games_id]["issue_date"] = ""
                self.games_dict[games_id]["Status"] = "Available"
                print("Game returned successfully!")
        else:
            print("Game ID not found.")

    try:
        myLMS = LMS("List_of_games.txt", "Python's Media Library")
        press_key_list = {"D": "Display Games", "I": "Issue Games", "A": "Add Games", "R": "Return Games", "Q": "Quit" }
        key_press = None

      while key_press != "q":
          print(f"\n----------------Welcome To {myLMS.medialibrary_name} Library Management System------------\n")
          for key, value in press_key_list.items():
              print(f"Press '{key}' to {value}")

          key_press = input("Press key: ").lower()
          if key_press == "i":
              print("\nCurrent Selection: Issue Games\n")
              myLMS.issue_games()
          elif key_press == "a":
              print("\nCurrent Selection: Add Games\n")
              myLMS.add_games()
          elif key_press == "d":
              print("\nCurrent Selection: Display Games\n")
              myLMS.display_games()
          elif key_press == "r":
              print("\nCurrent Selection: Return Games\n")
              myLMS.return_games()
          elif key_press == "q":
              break
          else:
              continue
      except Exception as e:
       print("Something went wrong. Please check your input!")
