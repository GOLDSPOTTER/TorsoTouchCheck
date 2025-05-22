using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TorsoTouchCheck : MonoBehaviour
{
    private List<Transform> robotPieces = new List<Transform>();
    private bool robotBuilt = false;

    private bool movingLeft = true;
    private int stepsTaken = 0;

    void Start()
    {
        foreach (Transform piece in this.transform)
        {
            robotPieces.Add(piece);
        }
    }

    void Update()
    {
        if (robotBuilt == true)
        {
            int movementDirection;
           
                if (movingLeft)
                {
                    movementDirection = -5;
                }
                else
                {
                    movementDirection = 5;
                }

                stepsTaken += 1;

                if (stepsTaken >= 20)
                {
                    movingLeft = !movingLeft;
                    stepsTaken = 0;
                }

                this.transform.position += new Vector3(0, 0, movementDirection) * Time.deltaTime;
        }
    }
    private void OnCollisionEnter(Collision collision)
    {
        string targetName = collision.gameObject.name;
        Transform pieceInRobot = robotPieces.Find(piece => piece.name == targetName);

        if (pieceInRobot != null)
        {
            pieceInRobot.gameObject.SetActive(true);
            Destroy(collision.gameObject);
            robotPieces.Remove(pieceInRobot);
            Debug.Log(robotPieces.Count);

            if (robotPieces.Count <= 1)
            {
                robotBuilt = true;
            }
        }
    }
}
