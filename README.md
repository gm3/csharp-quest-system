## Quest Manager System

This Quest Manager System provides an easy and flexible way to create and manage quests and acts in your game. It includes built-in features to handle a variety of quest types, actions, and rewards. The main components of this system are explained in detail below.

### Classes and Fields

#### Reward

This class handles different types of rewards in the game.

- `RewardType`: Enum with types of rewards: Gold, Item, Experience.
- `item`: Name of the item, if the reward is an item.
- `quantity`: Quantity of the reward.
- `Claim()`: Function to claim a reward.

#### QuestAction

This class handles different actions required to complete a quest.

- `ActionType`: Enum with types of actions: FindItem, BeatBoss, TalkToNPC, BuyItem.
- `title`, `description`, `hint`, `target`, `quantity`: Various properties of an action.
- `isComplete`: Indicates whether the action is complete.
- `CheckActionComplete()`: Function to check if an action is complete.

#### Quest

This class handles all the properties and actions of a quest.

- `QuestType`: Enum with types of quests: MainQuest, SideQuest.
- `name`, `title`, `description`: Various properties of a quest.
- `isFailable`, `isSkippable`, `isFailed`: Indicates various states of a quest.
- `prerequisites`: Prerequisite quests that must be completed before this quest can be started.
- `actions`: List of actions required to complete the quest.
- `rewards`: List of rewards for completing the quest.
- `isTimedQuest`, `duration`: Properties for timed quests.
- `IsComplete`, `Progress`: Properties to check if a quest is complete and to get the progress of the quest.
- `StartQuest()`, `FailQuest()`, `SkipQuest()`, `IsTimedOut()`: Various methods to manage a quest.

#### Act

This class groups related quests together.

- `name`: Name of the act.
- `quests`: List of quests in the act.
- `rewards`: List of rewards for completing all quests in the act.
- `IsComplete`, `Progress`: Properties to check if an act is complete and to get the progress of the act.

#### QuestManager

This class handles all quest and act operations in the game.

- `acts`: List of acts in the game.
- `failedQuests`: List of failed quests.
- `currentQuest`: The quest that is currently active.
- `MainQuestProgress`, `SideQuestProgress`: Properties to get the progress of all main quests and side quests.
- `FailCurrentQuest()`, `SkipCurrentQuest()`, `ArePrerequisitesMet()`, `ToggleQuest()`, `StartQuest()`, `CompleteAction()`: Various methods to manage quests.

## Main Classes

### Reward

This class represents a reward given to the player after completing a quest. It includes fields for the reward type (Gold, Item, Experience), the item name (if the reward is an item), and the quantity of the reward.

### QuestAction

This class represents a single action that the player must complete as part of a quest. It has fields for action type (FindItem, BeatBoss, TalkToNPC, BuyItem), the action title and description, a hint to help the player complete the action, the target of the action, the quantity of the target that must be completed, whether the action is complete, and the position in the game world related to the action.

### Quest

This class represents a quest. It includes several fields related to the quest itself (type, name, title, description, whether the quest can fail or be skipped, whether the quest has failed, prerequisites for the quest, position in the game world related to the quest), a list of QuestActions that make up the quest, a list of Rewards for completing the quest, and fields related to timed quests.

### Act

This class represents an act, which is a group of related quests. It includes a name for the act, a list of quests that make up the act, and a list of rewards for completing the act.

### QuestManager

This is the main class that manages all quests and acts in the game. It includes a list of acts, the currently active quest, and methods for handling quests and acts.

## Main Methods

### StartQuest

This method starts a quest given the name of the act and the name of the quest.

### CompleteAction

This method completes an action of the current quest given the action target and the quantity of the target.

### FailCurrentQuest

This method fails the current quest given a reason for the failure.

### SkipCurrentQuest

This method skips the current quest.

### ArePrerequisitesMet

This method checks whether the prerequisites for a quest are met.

### ToggleQuest

This method toggles the status of a quest given the name of the act and the name of the quest.

### Implementation Guide

1. Define your rewards: Create instances of the `Reward` class and define their type, item (if applicable), and quantity.

2. Define your actions: Create instances of the `QuestAction` class and define their type, title, description, hint, target, quantity, and position.

3. Define your quests: Create instances of the `Quest` class and define their type, name, title, description, failable/skippable status, prerequisites (if any), position, actions, rewards, timed status, and duration (if it's a timed quest). Then, you can use the provided methods to manage the quests.

4. Group your quests into acts: Create instances of the `Act` class and define their name and the quests they contain. You can also define rewards for completing all quests in the act.

5. Manage your quests and acts: Finally, use the `QuestManager` class to manage all the quests and acts in your game. The class provides various methods to check the progress of quests, fail or skip quests, check prerequisites, toggle quests, start quests, and complete actions.

### How to Use

- Start a quest: Call the `StartQuest` method with the act and quest name as arguments.
- Complete an action: Call the `CompleteAction` method with the action target and quantity as arguments.
- Toggle the status of a quest: Call the `ToggleQuest` method with the act and quest name as arguments.
- Check if prerequisites for a quest are met: Call the `ArePrerequisitesMet` method with the quest as an argument.
- Skip or fail the current quest: Call the `SkipCurrentQuest` or `FailCurrentQuest` method.
- Check the progress of all main quests or side quests: Access the `MainQuestProgress` or `SideQuestProgress` property.


## Usage

You would typically use the `QuestManager` class to manage your quests. Here's a brief tutorial:

- Define your acts, quests, actions, and rewards in your game setup code, and add them to the `QuestManager`.
- To start a quest, call `StartQuest` with the act name and quest name.
- As the player completes actions related to the quest, call `CompleteAction` with the action target and quantity.
- If you want to fail the current quest, call `FailCurrentQuest` with a reason for the failure.
- If you want to skip the current quest, call `SkipCurrentQuest`.
- If you want to check whether the prerequisites for a quest are met, call `ArePrerequisitesMet` with the quest.
- If you want to toggle the status of a quest, call `ToggleQuest` with the act name and quest name.


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Collections;

// Reward class to handle different types of rewards in the game
[System.Serializable]
public class Reward
{
    public enum RewardType { Gold, Item, Experience }

    public RewardType rewardType;
    public string item;
    public int quantity;

    // Function to claim a reward
    public void Claim()
    {
        Debug.Log($"Claimed {quantity} {rewardType}. Item: {item}");
    }
}

// QuestAction class to handle different actions required to complete a quest
[System.Serializable]
public class QuestAction
{
    public enum ActionType { FindItem, BeatBoss, TalkToNPC, BuyItem }

    public ActionType actionType;
    public string title;
    public string description;
    public string hint;
    public string target;
    public int quantity;
    public bool isComplete;
    public Transform position;
    public int quantityCompleted;

    // Function to check if a quest action is completed
    public bool CheckActionComplete(string target, int quantity)
    {
        if (this.target == target)
        {
            quantityCompleted += quantity;
            if (quantityCompleted >= this.quantity)
            {
                isComplete = true;
                Debug.Log($"QuestAction {actionType} complete.");
            }
        }
        return isComplete;
    }
}

// Main Quest class to handle all the properties and actions of a quest
[System.Serializable]
public class Quest
{
    public enum QuestType { MainQuest, SideQuest }

    public QuestType type;
    public string name;
    public string title;
    public string description;
    public bool isFailable;
    public bool isSkippable;
    public bool isFailed;
    public string[] prerequisites;
    public Transform position;
    public List<QuestAction> actions;
    public List<Reward> rewards; // Rewards for completing the quest
    public bool isTimedQuest;
    public float duration; // Duration of the quest in seconds
    private float startTime;

    // Property to check if all quest actions are complete
    public bool IsComplete => actions.All(a => a.isComplete);

    // Property to get the progress of the quest
    public float Progress => actions.Count == 0 ? 0 : (float)actions.Count(a => a.isComplete) / actions.Count;

    // Function to start a quest
    // Function to start a quest
    public void StartQuest(MonoBehaviour behaviour)
    {
        if (isTimedQuest)
        {
            StartTimedQuest(behaviour);
        }
    }

    // Function to start a timed quest
    private void StartTimedQuest(MonoBehaviour behaviour)
    {
        behaviour.StartCoroutine(QuestTimer());
    }

    // Function to check if a timed quest has timed out
    public bool IsTimedOut()
    {
        return isTimedQuest && Time.time - startTime > duration;
    }

    // Function to fail a quest
    public void FailQuest(string reason)
    {
        if (isFailable)
        {
            isFailed = true;
            Debug.Log($"Quest {name} failed. Reason: {reason}");
        }
    }

    // Function to skip a quest
    public void SkipQuest()
    {
        if (isSkippable)
        {
            actions.ForEach(a => a.isComplete = true);
            Debug.Log($"Quest {name} skipped.");
        }
    }




    private IEnumerator QuestTimer()
    {
        yield return new WaitForSeconds(duration);
        FailQuest("Time ran out.");
        Debug.Log($"Quest {name} has timed out.");
    }

}

// Act class to group related quests together
[System.Serializable]
public class Act
{
    public string name;
    public List<Quest> quests;
    public List<Reward> rewards;

    // Property to check if all quests in the act are complete
    public bool IsComplete => quests.All(quest => quest.IsComplete);

    // Property to get the progress of the act
    public float Progress => quests.Count == 0 ? 0 : (float)quests.Count(q => q.IsComplete) / quests.Count;
}

// Main QuestManager class to handle all quest and act operations in the game
public class QuestManager : MonoBehaviour
{
    public List<Act> acts = new List<Act>();
    public List<Quest> failedQuests = new List<Quest>();
    public Quest currentQuest;

    public event Action<Quest> OnQuestToggled;

    private void Start()
    {
        // Initialization code here
    }

    private void Update()
    {
        // Check for end of game
        if (acts.All(a => a.IsComplete))
        {
            Debug.Log("All acts complete. The game ends.");
        }

        // Check if the current quest has timed out or failed
        if (currentQuest != null)
        {
            if (currentQuest.IsTimedOut())
            {
                currentQuest.FailQuest("Time ran out.");
                Debug.Log($"Quest {currentQuest.name} has timed out.");
            }
            else if (currentQuest.isFailed)
            {
                Debug.Log($"Quest {currentQuest.name} has failed.");
                currentQuest = null;
            }
        }
    }

    // Property to get the progress of all main quests
    public float MainQuestProgress
    {
        get
        {
            var mainQuests = acts.SelectMany(a => a.quests).Where(q => q.type == Quest.QuestType.MainQuest).ToList();
            return mainQuests.Count == 0 ? 0 : (float)mainQuests.Count(q => q.IsComplete) / mainQuests.Count;
        }
    }

    // Property to get the progress of all side quests
    public float SideQuestProgress
    {
        get
        {
            var sideQuests = acts.SelectMany(a => a.quests).Where(q => q.type == Quest.QuestType.SideQuest).ToList();
            return sideQuests.Count == 0 ? 0 : (float)sideQuests.Count(q => q.IsComplete) / sideQuests.Count;
        }
    }

    // Function to fail the current quest
    public void FailCurrentQuest(string reason)
    {
        if (currentQuest != null)
        {
            currentQuest.FailQuest(reason);
            currentQuest = null;
        }
    }

    // Function to skip the current quest
    public void SkipCurrentQuest()
    {
        if (currentQuest != null)
        {
            currentQuest.SkipQuest();
            currentQuest = null;
        }
    }

    // Function to check if the prerequisites for a quest are met
    public bool ArePrerequisitesMet(Quest quest)
    {
        foreach (var prereq in quest.prerequisites)
        {
            var act = acts.Find(a => a.quests.Exists(q => q.name == prereq));
            if (act == null)
            {
                Debug.Log($"Prerequisite quest {prereq} not found in any act.");
                return false;
            }

            var prereqQuest = act.quests.Find(q => q.name == prereq);
            if (!prereqQuest.IsComplete)
            {
                Debug.Log($"Prerequisite quest {prereq} not complete.");
                return false;
            }
        }

        return true;
    }

    // Function to toggle the status of a quest
    public void ToggleQuest(string actName, string questName)
    {
        var act = acts.Find(a => a.name == actName);
        if (act != null)
        {
            var quest = act.quests.Find(q => q.name == questName);
            if (quest != null)
            {
                quest.actions.ForEach(a => a.isComplete = !a.isComplete);
                Debug.Log($"Toggled quest {questName} to {quest.IsComplete}");
                OnQuestToggled?.Invoke(quest);
            }
                        else
            {
                Debug.Log($"No quest named {questName} found in act {actName}.");
            }
        }
        else
        {
            Debug.Log($"No act named {actName} found.");
        }
    }

    public void StartQuest(string actName, string questName)
    {
        var act = acts.Find(a => a.name == actName);
        if (act != null)
        {
            var quest = act.quests.Find(q => q.name == questName);
            if (quest != null)
            {
                if (ArePrerequisitesMet(quest))
                {
                    quest.StartQuest(this);  // Passing in QuestManager as MonoBehaviour
                    currentQuest = quest;
                    Debug.Log($"Started quest {questName} in act {actName}.");
                }
                else
                {
                    Debug.Log($"Prerequisites for quest {questName} not met.");
                }
            }
            else
            {
                Debug.Log($"No quest named {questName} found in act {actName}.");
            }
        }
        else
        {
            Debug.Log($"No act named {actName} found.");
        }
    }


    // Function to complete an action of the current quest
    public void CompleteAction(string actionTarget, int quantity)
    {
        if (currentQuest != null)
        {
            var action = currentQuest.actions.Find(a => a.target == actionTarget);
            if (action != null)
            {
                if (action.CheckActionComplete(actionTarget, quantity))
                {
                    Debug.Log($"Action {action.actionType} completed for target {actionTarget}.");
                }
            }
            else
            {
                Debug.Log($"No action with target {actionTarget} found in current quest.");
            }
        }
        else
        {
            Debug.Log($"No current quest active.");
        }
    }

    

}


```

## Unreal WIP

```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"

// Reward class to handle different types of rewards in the game
UENUM(BlueprintType)
enum class ERewardType : uint8
{
	Gold,
	Item,
	Experience
};

USTRUCT(BlueprintType)
struct FReward
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	ERewardType RewardType;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Item;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	int32 Quantity;

	// Function to claim a reward
	UFUNCTION(BlueprintCallable)
	void Claim()
	{
		UE_LOG(LogTemp, Warning, TEXT("Claimed %d %s. Item: %s"), Quantity, GetRewardTypeAsString(), *Item);
	}

	FString GetRewardTypeAsString() const
	{
		static const UEnum* EnumPtr = FindObject<UEnum>(ANY_PACKAGE, TEXT("ERewardType"), true);
		if (!EnumPtr)
		{
			return FString();
		}
		return EnumPtr->GetNameStringByIndex(static_cast<int32>(RewardType));
	}
};

// QuestAction class to handle different actions required to complete a quest
UENUM(BlueprintType)
enum class EActionType : uint8
{
	FindItem,
	BeatBoss,
	TalkToNPC,
	BuyItem
};

USTRUCT(BlueprintType)
struct FQuestAction
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	EActionType ActionType;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Title;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Description;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Hint;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Target;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	int32 Quantity;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	bool IsComplete;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FTransform Position;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	int32 QuantityCompleted;

	// Function to check if a quest action is completed
	bool CheckActionComplete(const FString& InTarget, int32 InQuantity)
	{
		if (Target == InTarget)
		{
			QuantityCompleted += InQuantity;
			if (QuantityCompleted >= Quantity)
			{
				IsComplete = true;
				UE_LOG(LogTemp, Warning, TEXT("QuestAction %s complete."), GetActionTypeAsString());
			}
		}
		return IsComplete;
	}

	FString GetActionTypeAsString() const
	{
		static const UEnum* EnumPtr = FindObject<UEnum>(ANY_PACKAGE, TEXT("EActionType"), true);
		if (!EnumPtr)
		{
			return FString();
		}
		return EnumPtr->GetNameStringByIndex(static_cast<int32>(ActionType));
	}
};

// Main Quest class to handle all the properties and actions of a quest
UENUM(BlueprintType)
enum class EQuestType : uint8
{
	MainQuest,
	SideQuest
};

USTRUCT(BlueprintType)
struct FQuest
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	EQuestType Type;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Name;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Title;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Description;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	bool IsFailable;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	bool IsSkippable;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	bool IsFailed;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FString> Prerequisites;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FTransform Position;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FQuestAction> Actions;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FReward> Rewards;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	bool IsTimedQuest;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	float Duration;

private:
	float StartTime;

public:
	// Property to check if all quest actions are complete
	bool IsComplete() const
	{
		for (const FQuestAction& Action : Actions)
		{
			if (!Action.IsComplete)
			{
				return false;
			}
		}
		return true;
	}

	// Property to get the progress of the quest
	float GetProgress() const
	{
		const int32 NumActions = Actions.Num();
		if (NumActions == 0)
		{
			return 0.0f;
		}
		int32 NumCompletedActions = 0;
		for (const FQuestAction& Action : Actions)
		{
			if (Action.IsComplete)
			{
				NumCompletedActions++;
			}
		}
		return static_cast<float>(NumCompletedActions) / NumActions;
	}

	// Function to start a quest
	void StartQuest(AActor* Owner)
	{
		if (IsTimedQuest)
		{
			StartTimedQuest(Owner);
		}
	}

	// Function to start a timed quest
	void StartTimedQuest(AActor* Owner)
	{
		Owner->GetWorldTimerManager().SetTimerForNextTick(FTimerDelegate::CreateUObject(this, &FQuest::QuestTimer));
	}

	// Function to check if a timed quest has timed out
	bool IsTimedOut() const
	{
		return IsTimedQuest && (FPlatformTime::Seconds() - StartTime) > Duration;
	}

	// Function to fail a quest
	void FailQuest(const FString& Reason)
	{
		if (IsFailable)
		{
			IsFailed = true;
			UE_LOG(LogTemp, Warning, TEXT("Quest %s failed. Reason: %s"), *Name, *Reason);
		}
	}

	// Function to skip a quest
	void SkipQuest()
	{
		if (IsSkippable)
		{
			for (FQuestAction& Action : Actions)
			{
				Action.IsComplete = true;
			}
			UE_LOG(LogTemp, Warning, TEXT("Quest %s skipped."), *Name);
		}
	}

private:
	void QuestTimer()
	{
		FailQuest("Time ran out.");
		UE_LOG(LogTemp, Warning, TEXT("Quest %s has timed out."), *Name);
	}
};

// Act class to group related quests together
USTRUCT(BlueprintType)
struct FAct
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString Name;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FQuest> Quests;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FReward> Rewards;

	// Property to check if all quests in the act are complete
	bool IsComplete() const
	{
		for (const FQuest& Quest : Quests)
		{
			if (!Quest.IsComplete())
			{
				return false;
			}
		}
		return true;
	}

	// Property to get the progress of the act
	float GetProgress() const
	{
		const int32 NumQuests = Quests.Num();
		if (NumQuests == 0)
		{
			return 0.0f;
		}
		int32 NumCompletedQuests = 0;
		for (const FQuest& Quest : Quests)
		{
			if (Quest.IsComplete())
			{
				NumCompletedQuests++;
			}
		}
		return static_cast<float>(NumCompletedQuests) / NumQuests;
	}
};

// Main QuestManager class to handle all quest and act operations in the game
UCLASS()
class AQuestManager : public AActor
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FAct> Acts;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<FQuest> FailedQuests;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FQuest CurrentQuest;

	// Event delegate for quest toggling
	DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FQuestToggledDelegate, const FQuest&, Quest);
	UPROPERTY(BlueprintAssignable)
	FQuestToggledDelegate OnQuestToggled;

	// Function to fail the current quest
	UFUNCTION(BlueprintCallable)
	void FailCurrentQuest(const FString& Reason)
	{
		if (CurrentQuest.IsFailed)
		{
			CurrentQuest.FailQuest(Reason);
			CurrentQuest = FQuest();
		}
	}

	// Function to skip the current quest
	UFUNCTION(BlueprintCallable)
	void SkipCurrentQuest()
	{
		if (CurrentQuest.IsSkippable)
		{
			CurrentQuest.SkipQuest();
			CurrentQuest = FQuest();
		}
	}

	// Function to check if the prerequisites for a quest are met
	UFUNCTION(BlueprintCallable)
	bool ArePrerequisitesMet(const FQuest& Quest)
	{
		for (const FString& Prerequisite : Quest.Prerequisites)
		{
			const FQuest* PrerequisiteQuest = FindQuestByName(Prerequisite);
			if (PrerequisiteQuest == nullptr)
			{
				UE_LOG(LogTemp, Warning, TEXT("Prerequisite quest %s not found in any act."), *Prerequisite);
				return false;
			}
			if (!PrerequisiteQuest->IsComplete())
			{
				UE_LOG(LogTemp, Warning, TEXT("Prerequisite quest %s not complete."), *Prerequisite);
				return false;
			}
		}
		return true;
	}

	// Function to toggle the status of a quest
	UFUNCTION(BlueprintCallable)
	void ToggleQuest(const FString& ActName, const FString& QuestName)
	{
		FAct* Act = FindActByName(ActName);
		if (Act)
		{
			FQuest* Quest = FindQuestByNameInAct(*Act, QuestName);
			if (Quest)
			{
				for (FQuestAction& Action : Quest->Actions)
				{
					Action.IsComplete = !Action.IsComplete;
				}
				UE_LOG(LogTemp, Warning, TEXT("Toggled quest %s to %d"), *QuestName, Quest->IsComplete());
				OnQuestToggled.Broadcast(*Quest);
			}
			else
			{
				UE_LOG(LogTemp, Warning, TEXT("No quest named %s found in act %s."), *QuestName, *ActName);
			}
		}
		else
		{
			UE_LOG(LogTemp, Warning, TEXT("No act named %s found."), *ActName);
		}
	}

	// Function to start a quest
	UFUNCTION(BlueprintCallable)
	void StartQuest(const FString& ActName, const FString& QuestName)
	{
		FAct* Act = FindActByName(ActName);
		if (Act)
		{
			FQuest* Quest = FindQuestByNameInAct(*Act, QuestName);
			if (Quest)
			{
				if (ArePrerequisitesMet(*Quest))
				{
					Quest->StartQuest(this);
					CurrentQuest = *Quest;
					UE_LOG(LogTemp, Warning, TEXT("Started quest %s in act %s."), *QuestName, *ActName);
				}
				else
				{
					UE_LOG(LogTemp, Warning, TEXT("Prerequisites for quest %s not met."), *QuestName);
				}
			}
			else
			{
				UE_LOG(LogTemp, Warning, TEXT("No quest named %s found in act %s."), *QuestName, *ActName);
			}
		}
		else
		{
			UE_LOG(LogTemp, Warning, TEXT("No act named %s found."), *ActName);
		}
	}

	// Function to complete an action of the current quest
	UFUNCTION(BlueprintCallable)
	void CompleteAction(const FString& ActionTarget, int32 Quantity)
	{
		if (CurrentQuest.IsFailable)
		{
			FQuestAction* Action = FindActionByTarget(CurrentQuest, ActionTarget);
			if (Action)
			{
				if (Action->CheckActionComplete(ActionTarget, Quantity))
				{
					UE_LOG(LogTemp, Warning, TEXT("Action %s completed for target %s."), *Action->GetActionTypeAsString(), *ActionTarget);
				}
			}
			else
			{
				UE_LOG(LogTemp, Warning, TEXT("No action with target %s found in current quest."), *ActionTarget);
			}
		}
		else
		{
			UE_LOG(LogTemp, Warning, TEXT("No current quest active."));
		}
	}

private:
	// Helper function to find an act by name
	FAct* FindActByName(const FString& ActName)
	{
		for (FAct& Act : Acts)
		{
			if (Act.Name == ActName)
			{
				return &Act;
			}
		}
		return nullptr;
	}

	// Helper function to find a quest by name in an act
	FQuest* FindQuestByNameInAct(const FAct& Act, const FString& QuestName)
	{
		for (FQuest& Quest : Act.Quests)
		{
			if (Quest.Name == QuestName)
			{
				return &Quest;
			}
		}
		return nullptr;
	}

	// Helper function to find a quest by name
	FQuest* FindQuestByName(const FString& QuestName)
	{
		for (FAct& Act : Acts)
		{
			FQuest* Quest = FindQuestByNameInAct(Act, QuestName);
			if (Quest)
			{
				return Quest;
			}
		}
		return nullptr;
	}

	// Helper function to find an action by target in a quest
	FQuestAction* FindActionByTarget(const FQuest& Quest, const FString& ActionTarget)
	{
		for (FQuestAction& Action : Quest.Actions)
		{
			if (Action.Target == ActionTarget)
			{
				return &Action;
			}
		}
		return nullptr;
	}
};

```

## Three.js WIP

```javascript=
// Reward class to handle different types of rewards in the game
class Reward {
  constructor() {
    this.rewardType;
    this.item;
    this.quantity;
  }

  // Function to claim a reward
  claim() {
    console.log(`Claimed ${this.quantity} ${this.rewardType}. Item: ${this.item}`);
  }
}

// QuestAction class to handle different actions required to complete a quest
class QuestAction {
  constructor() {
    this.actionType;
    this.title;
    this.description;
    this.hint;
    this.target;
    this.quantity;
    this.isComplete;
    this.position;
    this.quantityCompleted;
  }

  // Function to check if a quest action is completed
  checkActionComplete(target, quantity) {
    if (this.target === target) {
      this.quantityCompleted += quantity;
      if (this.quantityCompleted >= this.quantity) {
        this.isComplete = true;
        console.log(`QuestAction ${this.actionType} complete.`);
      }
    }
    return this.isComplete;
  }
}

// Main Quest class to handle all the properties and actions of a quest
class Quest {
  constructor() {
    this.type;
    this.name;
    this.title;
    this.description;
    this.isFailable;
    this.isSkippable;
    this.isFailed;
    this.prerequisites;
    this.position;
    this.actions;
    this.rewards;
    this.isTimedQuest;
    this.duration;
    this.startTime;
  }

  // Property to check if all quest actions are complete
  get isComplete() {
    return this.actions.every(a => a.isComplete);
  }

  // Property to get the progress of the quest
  get progress() {
    return this.actions.length === 0 ? 0 : this.actions.filter(a => a.isComplete).length / this.actions.length;
  }

  // Function to start a quest
  startQuest(behaviour) {
    if (this.isTimedQuest) {
      this.startTimedQuest(behaviour);
    }
  }

  // Function to start a timed quest
  startTimedQuest(behaviour) {
    // Implement the timer logic using setTimeout or setInterval in JavaScript
    console.log('Starting timed quest...');
  }

  // Function to check if a timed quest has timed out
  isTimedOut() {
    return this.isTimedQuest && Date.now() - this.startTime > this.duration * 1000;
  }

  // Function to fail a quest
  failQuest(reason) {
    if (this.isFailable) {
      this.isFailed = true;
      console.log(`Quest ${this.name} failed. Reason: ${reason}`);
    }
  }

  // Function to skip a quest
  skipQuest() {
    if (this.isSkippable) {
      this.actions.forEach(a => (a.isComplete = true));
      console.log(`Quest ${this.name} skipped.`);
    }
  }

  // Function to handle quest timer
  questTimer() {
    // Implement the timer logic for the quest using setTimeout or setInterval in JavaScript
    console.log(`Quest ${this.name} has timed out.`);
  }
}

// Act class to group related quests together
class Act {
  constructor() {
    this.name;
    this.quests;
    this.rewards;
  }

  // Property to check if all quests in the act are complete
  get isComplete() {
    return this.quests.every(quest => quest.isComplete);
  }

  // Property to get the progress of the act
  get progress() {
    return this.quests.length === 0 ? 0 : this.quests.filter(quest => quest.isComplete).length / this.quests.length;
  }
}

// QuestManager class to handle all quest and act operations in the game
class QuestManager {
  constructor() {
    this.acts = [];
    this.failedQuests = [];
    this.currentQuest;
  }

  // Initialization function
  init() {
    // Initialization code here
  }

  // Update function
  update() {
    // Check for end of game
    if (this.acts.every(a => a.isComplete)) {
      console.log('All acts complete. The game ends.');
    }

    // Check if the current quest has timed out or failed
    if (this.currentQuest) {
      if (this.currentQuest.isTimedOut()) {
        this.currentQuest.failQuest('Time ran out.');
        console.log(`Quest ${this.currentQuest.name} has timed out.`);
      } else if (this.currentQuest.isFailed) {
        console.log(`Quest ${this.currentQuest.name} has failed.`);
        this.currentQuest = null;
      }
    }
  }

  // Property to get the progress of all main quests
  get mainQuestProgress() {
    const mainQuests = this.acts
      .flatMap(a => a.quests)
      .filter(q => q.type === Quest.QuestType.MainQuest);
    return mainQuests.length === 0 ? 0 : mainQuests.filter(q => q.isComplete).length / mainQuests.length;
  }

  // Property to get the progress of all side quests
  get sideQuestProgress() {
    const sideQuests = this.acts
      .flatMap(a => a.quests)
      .filter(q => q.type === Quest.QuestType.SideQuest);
    return sideQuests.length === 0 ? 0 : sideQuests.filter(q => q.isComplete).length / sideQuests.length;
  }

  // Function to fail the current quest
  failCurrentQuest(reason) {
    if (this.currentQuest) {
      this.currentQuest.failQuest(reason);
      this.currentQuest = null;
    }
  }

  // Function to skip the current quest
  skipCurrentQuest() {
    if (this.currentQuest) {
      this.currentQuest.skipQuest();
      this.currentQuest = null;
    }
  }

  // Function to check if the prerequisites for a quest are met
  arePrerequisitesMet(quest) {
    for (const prereq of quest.prerequisites) {
      const act = this.acts.find(a => a.quests.some(q => q.name === prereq));
      if (!act) {
        console.log(`Prerequisite quest ${prereq} not found in any act.`);
        return false;
      }

      const prereqQuest = act.quests.find(q => q.name === prereq);
      if (!prereqQuest.isComplete) {
        console.log(`Prerequisite quest ${prereq} not complete.`);
        return false;
      }
    }

    return true;
  }

  // Function to toggle the status of a quest
  toggleQuest(actName, questName) {
    const act = this.acts.find(a => a.name === actName);
    if (act) {
      const quest = act.quests.find(q => q.name === questName);
      if (quest) {
        quest.actions.forEach(a => (a.isComplete = !a.isComplete));
        console.log(`Toggled quest ${questName} to ${quest.isComplete}`);
        this.onQuestToggled(quest);
      } else {
        console.log(`No quest named ${questName} found in act ${actName}.`);
      }
    } else {
      console.log(`No act named ${actName} found.`);
    }
  }

  // Function to start a quest
  startQuest(actName, questName) {
    const act = this.acts.find(a => a.name === actName);
    if (act) {
      const quest = act.quests.find(q => q.name === questName);
      if (quest) {
        if (this.arePrerequisitesMet(quest)) {
          quest.startQuest(this); // Passing in QuestManager as the behaviour
          this.currentQuest = quest;
          console.log(`Started quest ${questName} in act ${actName}.`);
        } else {
          console.log(`Prerequisites for quest ${questName} not met.`);
        }
      } else {
        console.log(`No quest named ${questName} found in act ${actName}.`);
      }
    } else {
      console.log(`No act named ${actName} found.`);
    }
  }

  // Function to complete an action of the current quest
  completeAction(actionTarget, quantity) {
    if (this.currentQuest) {
      const action = this.currentQuest.actions.find(a => a.target === actionTarget);
      if (action) {
        if (action.checkActionComplete(actionTarget, quantity)) {
          console.log(`Action ${action.actionType} completed for target ${actionTarget}.`);
        }
      } else {
        console.log(`No action with target ${actionTarget} found in the current quest.`);
      }
    } else {
      console.log('No current quest active.');
    }
  }

  // Event handler for quest toggling
  onQuestToggled(quest) {
    console.log(`Quest ${quest.name} toggled.`);
  }
}

```
