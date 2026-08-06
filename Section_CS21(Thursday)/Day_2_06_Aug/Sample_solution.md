    /**
     * Q1: Finds roll numbers of students present in BOTH sessions.
     *
     * Both arrays are already sorted (smallest to largest).
     * We walk through both arrays at the same time using two counters
     * (i for morningSession, j for afternoonSession), instead of
     * comparing every element of one array with every element of the
     * other (which would be slower).
     *
     * Time Complexity  : O(m + n)  -> we only pass through each array once
     * Space Complexity : O(1) extra (not counting the array we return)
     */
	 
	FUNCTION findCommonRollNumbers(morningSession, afternoonSession):
    CREATE temp array of size = length(morningSession)
    SET count = 0
    SET i = 0            // pointer into morningSession
    SET j = 0            // pointer into afternoonSession

    WHILE i < length(morningSession) AND j < length(afternoonSession):
        IF morningSession[i] == afternoonSession[j] THEN
            temp[count] = morningSession[i]
            count = count + 1
            i = i + 1
            j = j + 1
        ELSE IF morningSession[i] < afternoonSession[j] THEN
            i = i + 1     // morning value too small, can't match anything later
        ELSE
            j = j + 1     // afternoon value too small, advance it instead
        END IF
    END WHILE

    CREATE result array of size = count
    FOR k = 0 TO count - 1:
        result[k] = temp[k]
    END FOR

    RETURN result
	END FUNCTION
	 
	 
	/**
     * Q2: Finds the length of the longest run of days where consumption
     * kept increasing every day.
     *
     * We go through the array once. "current" counts how long the
     * increasing run happening right now is. "longest" remembers the
     * best run we have seen so far.
     *
     * Time Complexity  : O(n) -> one pass through the array
     * Space Complexity : O(1) -> only two counters are used
     */
	 
	FUNCTION longestIncreasingStreak(consumption):
    IF length(consumption) == 0 THEN
        RETURN 0
    END IF

    SET longest = 1
    SET current = 1

    FOR day = 1 TO length(consumption) - 1:
        IF consumption[day] > consumption[day - 1] THEN
            current = current + 1
            IF current > longest THEN
                longest = current
            END IF
        ELSE
            current = 1    // streak broken, restart from this day
        END IF
    END FOR

    RETURN longest
	END FUNCTION
