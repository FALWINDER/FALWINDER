import unittest

# Sample scoring logic for the 10-pin bowling game
class BowlingGame:
    def __init__(self):
        self.rolls = []

    def roll(self, pins):
        self.rolls.append(pins)

    def score(self):
        score = 0
        frame_index = 0
        for frame in range(10):
            if self.is_strike(frame_index):  # Strike
                score += 10 + self.rolls[frame_index + 1] + self.rolls[frame_index + 2]
                frame_index += 1
            elif self.is_spare(frame_index):  # Spare
                score += 10 + self.rolls[frame_index + 2]
                frame_index += 2
            else:  # Open Frame
                score += self.rolls[frame_index] + self.rolls[frame_index + 1]
                frame_index += 2
        return score

    def is_strike(self, frame_index):
        return self.rolls[frame_index] == 10

    def is_spare(self, frame_index):
        return self.rolls[frame_index] + self.rolls[frame_index + 1] == 10


class TestBowlingGame(unittest.TestCase):
    def setUp(self):
        self.game = BowlingGame()

    def roll_many(self, n, pins):
        for _ in range(n):
            self.game.roll(pins)

    def roll_spare(self):
        self.game.roll(5)
        self.game.roll(5)

    def roll_strike(self):
        self.game.roll(10)

    # Test case 1: All gutter balls (score should be 0)
    def test_gutter_game(self):
        self.roll_many(20, 0)
        self.assertEqual(self.game.score(), 0)

    # Test case 2: All ones (score should be 20)
    def test_all_ones(self):
        self.roll_many(20, 1)
        self.assertEqual(self.game.score(), 20)

    # Test case 3: One spare (score should be 16)
    def test_one_spare(self):
        self.roll_spare()
        self.game.roll(3)
        self.roll_many(17, 0)
        self.assertEqual(self.game.score(), 16)

    # Test case 4: One strike (score should be 24)
    def test_one_strike(self):
        self.roll_strike()
        self.game.roll(3)
        self.game.roll(4)
        self.roll_many(16, 0)
        self.assertEqual(self.game.score(), 24)

    # Test case 5: Perfect game (score should be 300)
    def test_perfect_game(self):
        self.roll_many(12, 10)
        self.assertEqual(self.game.score(), 300)

if __name__ == '__main__':
    unittest.main(argv=[''], exit=False)
