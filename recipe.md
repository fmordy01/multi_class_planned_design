{{PROBLEM}} Multi-Class Planned Design Recipe
1. Describe the Problem
Put or write the user story here. Add any clarifying notes you might have.

    <!-- As a user
    So that I can record my experiences
    I want to keep a regular diary

    As a user
    So that I can reflect on my experiences
    I want to read my past diary entries

    As a user
    So that I can reflect on my experiences in my busy day
    I want to select diary entries to read based on how much time I have and my reading speed

    As a user
    So that I can keep track of my tasks
    I want to keep a todo list along with my diary

        * add todos, mark_complete, list complete, incomplete

    As a user
    So that I can keep track of my contacts
    I want to see a list of all of the mobile phone numbers in all my diary entries 
    
        *A phone number starts with zero and is 11 digits long

2. Design the Class System
Consider diagramming out the classes and their relationships. Take care to focus on the details you see as important, not everything. The diagram below uses asciiflow.com but you could also use excalidraw.com, draw.io, or miro.com

    #Verbs 
        Record 
        Keep
        Reflect
        Read
        Select
        Keep
        See a list
        Mark complete
        List complete
        List incomplete
        Add

    #Nouns
        Diary
        Diary Entries
        Experiences
        Time
        Reading speed
        Todo List
        Tasks
        Phone number
        List of Phone Numbers

class Diary():
    def add(self, diary_entry):
        diary_entry: instance of DiaryEntry
        returns nothing
        side effects: adds to list of diary entries 
        pass

    def all(self):
        returns a list of DiaryEntry instances
        pass

class DiaryEntry():
    public properties:
    title: a string representing an entry title
    contents: a string representing entry contents
    
    def __init__(self, title, contents):
        title: a string representing an entry title
        contents: a string representing entry contents 
        side effects: sets the above properties
        pass

class TaskList():
    def add(self, task):
        task: an instance of Task
        returns: nothing
        side effects: adds to list of tasks
        pass
    
    def all_incomplete(self):
        returns a list of instances of Task
        representing the incomplete tasks
        pass
    
    def all_complete(self):
        returns a list of instances of Task
        representing the complete tasks
        pass

class Task():
    public properties:
    title: representing a job to do - string

    def __init__(self, title):
        title: a string representing a job to do
        side effects: sets title property
        pass

    def mark_complete(self):
        side effects: marks the task as complete
        returns nothing
        pass

class PhoneNumberExtractor():
    def __init__(self, diary):
        diary: an instance of Diary
        side effects: set diary property
        pass

    def extract(self):
        returns a list of strings representing a list of numbers

class ReadableEntryExtractor():
    def __init__(self, diary):
        diary: an instance of Diary
        side effects: set diary property
        pass

    def extract(self):
        wmp: integer
        minutes: integer
        returns the longest diary entry that can be read given wpm, minutes
        pass



    def __init__(self, title):
    pass
    



3. Create Examples as Integration Tests
Create examples of the classes being used together in different situations and combinations that reflect the ways in which the system will be used.

'''python
"""
When I add multiple diary entries
#all lists them out in order they were added
"""

diary = Diary()
diary_entry_1 = DiaryEntry("My Title 1", "My Contents 1")
diary_entry_2 = DiaryEntry("My Title 2", "My Contents 2")
diary_entry_3 = DiaryEntry("My Title 3", "My Contents 3")
diary.add(diary_entry_1)
diary.add(diary_entry_2)
diary.add(diary_entry_3)
diary.all() # => [diary_entry_1, diary_entry_2, diary_entry_3]


"""
When adding multiple tasks 
None are marked complete
#all_incomplete lists them out in order they were added
"""

task_list = TaskList()
task_1 = Task("Walk the dog")
task_2 = Task("Walk the cat")
task_3 = Task("Walk the frog")
task_list.add(task_1)
task_list.add(task_2)
task_list.add(task_3)
task_list.all_incomplete() # => [task_1, task_2, task_3]

"""
When adding multiple tasks 
One marked as complete
#all_incomplete lists tasks that are not complete only
"""

task_list = TaskList()
task_1 = Task("Walk the dog")
task_2 = Task("Walk the cat")
task_3 = Task("Walk the frog")
task_list.add(task_1)
task_list.add(task_2)
task_list.add(task_3)
task_2.mark_complete()
task_list.all_incomplete() # => [task_1, task_3]


"""
When I add multiple diary entries
PhoneNumberExtraxtor #extract
List of all phone number from all diary entries
"""

diary = Diary()
diary_entry_1 = DiaryEntry("My Title 1", "My friend is 07800000000 and 07800000001")
diary_entry_2 = DiaryEntry("My Title 2", "My Contents 2")
diary_entry_3 = DiaryEntry("My Title 3", "My friend is 07800000002")
diary.add(diary_entry_1)
diary.add(diary_entry_2)
diary.add(diary_entry_3)
extractor = PhoneNumberExtractor(diary)
extractor.extract() # => [07800000000, 07800000001, 07800000002]

"""
When I add multiple diary entries
PhoneNumberExtraxtor #extract
It ignores invalid phone numbers
"""

diary = Diary()
diary_entry_1 = DiaryEntry("My Title 1", "My friend is 078000000000 and 0780000000 and 0780, 13141")
diary.add(diary_entry_1)
extractor = PhoneNumberExtractor(diary)
extractor.extract() # => []

"""
When I add a diary entry
PhoneNumberExtraxtor #extract
Ignores duplicates of numbers
"""

diary = Diary()
diary_entry_1 = DiaryEntry("My Title 1", "My friend is 07800000000 and 07800000000")
diary_entry_2 = DiaryEntry("My Title 2", "My friend is 07800000000")
diary.add(diary_entry_1)
diary.add(diary_entry_2)
extractor = PhoneNumberExtractor(diary)
extractor.extract() # => [07800000000]

"""
When I add no diary entries
PhoneNumberExtraxtor #extract
Returns []
"""

diary = Diary()
extractor = PhoneNumberExtractor(diary)
extractor.extract() # => []

"""
When adding one diary entry that fits in the time
And I call ReadableEntryExtractor
With wpm of 2 and minutes of 2
It gets that diary entry
"""

diary = Diary()
diary_entry_1 = DiaryEntry("Title 1", "one two three four")
diary.add(diary_entry_1)
extractor = ReadableEntryExtractor(diary)
extractor.extract(wpm=2, minutes=2) # => diary_entry_1


"""
When adding one diary entry that doesnt fits in the time
And I call ReadableEntryExtractor
With wpm of 2 and minutes of 2
returns none
"""

diary = Diary()
diary_entry_1 = DiaryEntry("Title 1", "one two three four five")
diary.add(diary_entry_1)
extractor = ReadableEntryExtractor(diary)
extractor.extract(wpm=2, minutes=2) # => None


"""
When adding multiple diary entries, one readable 
And I call ReadableEntryExtractor
With wpm of 2 and minutes of 2
returns readable entry 
"""

diary = Diary()
diary_entry_1 = DiaryEntry("Title 1", "one two three four five")
diary_entry_2 = DiaryEntry("Title 1", "one two three four")
diary.add(diary_entry_1)
diary.add(diary_entry_2)
extractor = ReadableEntryExtractor(diary)
extractor.extract(wpm=2, minutes=2) # => diary_entry_2

"""
When adding multiple diary entries, multiple readable 
And I call ReadableEntryExtractor
With wpm of 2 and minutes of 2
returns readable longest
"""

diary = Diary()
diary_entry_1 = DiaryEntry("Title 1", "one two three four five")
diary_entry_2 = DiaryEntry("Title 1", "one two three four")
diary_entry_3 = DiaryEntry("Title 1", "one two three")
diary.add(diary_entry_1)
diary.add(diary_entry_2)
diary.add(diary_entry_3)
extractor = ReadableEntryExtractor(diary)
extractor.extract(wpm=2, minutes=2) # => diary_entry_2

"""
When adding no diary entries
And I call ReadableEntryExtractor
returns None
"""

diary = Diary()
extractor = ReadableEntryExtractor(diary)
extractor.extract(wpm=2, minutes=2) # => None





4. Create Examples as Unit Tests
Create examples, where appropriate, of the behaviour of each relevant class at a more granular level of detail.

#Diary
"""
Initially Diary doesnt have an entry
"""

diary = Diary()
diary.all() #=> []

#DiaryEntry
entry = DiaryEntry("My title", "My Contents")
entry.title # => "My Title"
entry.contents # => "My Contents"

#TaskList
"""
Initially, TaskList has no incomplete tasks
"""

task_list = TaskList()
task_list.all_incomplete() # => []

#TaskList
"""
Initially, TaskList has no complete tasks
"""

task_list = TaskList()
task_list.all_complete() # => []

#Task

"""
Task constructs with a title
"""
task = Task("Walk the dog")
task.title # => "Walk the dog"




5. Implement the Behaviour
After each test you write, follow the test-driving process of red, green, refactor to implement the behaviour.