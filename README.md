# Input System Template for Unity 2019.4+

- A simple template for using the new Input System of Unity.
- This is useful to save some time when configuring a new project that requires a controller. 
- It will allow you to focus in developing your mechanics and not mix gameplay code with input code. :star_struck::star_struck:

### How to use

1 - Make sure you imported the new Input System package from the Package Manager

2 - Go to **Edit -> Project Settings -> Other Settings -> Active Input Handling** (Choose new Input System)

3 - Just for safety, make your API Compatibility Level to be .NET 4.x

4 - Create a player controller class and a movement function like in the example

```cs
using Buriola.InputSystem;
using CallbackContext = UnityEngine.InputSystem.InputActions.CallbackContext;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    private float _xMovement;
    private float _yMovement;

    void Start()
    {
        InputController.Instance.GameInputContext.OnMovementStart += MovementStart;
        InputController.Instance.GameInputContext.OnMovementEnded += MovementEnd;
    }
    
    void Update()
    {
        if(_xMovement != 0f || _yMovement != 0f)
        {
            //Handle your movement behavior
        }
    }
    
    void MovementStart(CallbackContext context)
    {
        var movement = context.ReadValue<Vector2>();
        
        _xMovement = movement.x;
        _yMovement = movement.y;
    }
    
    void MovementEnd(CallbackContext context)
    {
        _xMovement = 0f;
        _yMovement = 0f;
    }
    
    void OnDestroy()
    {
        InputController.Instance.GameInputContext.OnMovementStart -= MovementStart;
        InputController.Instance.GameInputContext.OnMovementEnded -= MovementEnd;
    }
}
```

You can edit the InputAsset to bind custom keyboard keys to whatever action you want or even create new Action Maps to use in different contexts.

### Current Architecture

- InputController.cs
  - This is a singleton and it should be accessible to all classes that you want to register input actions
- InGameInputContext.cs
  - This is a context that binds all inputs related to the InGame action map configured in the InputAsset
- UIInputContext.cs
  - This is a context that binds all inputs related to the UI action map configured in the InputAsset
  
You can create more action maps as needed.

### Next Improvements

- Make it possible to detect device changes. Whether the player switched to the keyboard or controller. This will require manual binding and pairing of the devices.
