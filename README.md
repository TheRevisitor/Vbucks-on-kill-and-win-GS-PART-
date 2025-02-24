# Vbucks-on-kill-and-win-GS-PART-
This was made for Reboot 3.0 and reload backend
(Reboot 3.0 made by Milxnor and reload backend was made by Lawin and Burlone)

# CREDIT ME IF YOU USE THIS!!!

Once you have the Reboot 3.0 source (which you get from https://github.com/Milxnor/Project-Reboot-3.0) just open the sln using visual studio 2022

Search for a file named FortPlayerController.cpp
![image](https://github.com/user-attachments/assets/9a769355-96dd-4f22-90ba-00be93dec689)


Once there search for "if (KillerPlayerState && KillerPlayerState != DeadPlayerState)" inside of that, make some space and paste this in:

    std::string username = KillerPlayerState->GetPlayerName().ToString(); 
    std::string apiKey = "ur-api-key";
    std::string reason = "Kill";
    std::string apiUrl = "http://127.0.0.1:3551/api/reload/vbucks?apikey=" + apiKey +
        "&username=" + username + "&reason=" + reason;

    CURL* curl;
    CURLcode res;

    curl = curl_easy_init();
    if (curl)
    {
        curl_easy_setopt(curl, CURLOPT_URL, apiUrl.c_str());
        curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);

        res = curl_easy_perform(curl);
        if (res != CURLE_OK)
        {
            LOG_ERROR(LogDev, "CURL request failed for {}: {}", killerUsername, curl_easy_strerror(res));
        }
        else
        {
            LOG_INFO(LogDev, "Successfully added V-Bucks for {}", killerUsername);
        }

        curl_easy_cleanup(curl);
    }
    else
    {
        LOG_ERROR(LogDev, "Failed to initialize CURL for {}.", killerUsername);
    }


wow! now you have vbucks on kill, just make sure to use the correct ip, port and api key


# Vbucks on win

in the same file (FortPlayerController.cpp) search for "void AFortPlayerController::ClientOnPawnDiedHook(AFortPlayerController* PlayerController, void* DeathReport)"
under all that stuff
![image](https://github.com/user-attachments/assets/3316564c-1a61-4505-a289-f863614b3d88)
make some space and paste this in:


auto AllPlayerStates = UGameplayStatics::GetAllActorsOfClass(GetWorld(), AFortPlayerStateAthena::StaticClass()); 

for (int i = 0; i < AllPlayerStates.Num(); ++i) {

	auto CurrentPlayerState = Cast<AFortPlayerStateAthena>(AllPlayerStates.At(i));
	if (!CurrentPlayerState) continue;
	auto ControllerOwner = CurrentPlayerState->GetOwner();

	if (ControllerOwner && ControllerOwner->IsA(AFortPlayerControllerAthena::StaticClass()) && CurrentPlayerState->GetPlace() == 1) {
		std::string PlayerName = CurrentPlayerState->GetPlayerName().ToString();
		std::string apiKey = "ur-api-key";
		std::string reason = "Win";

		std::string apiUrl1 = "http://127.0.0.1:3551/api/reload/vbucks?apikey=" + apiKey +
			"&username=" + PlayerName + "&reason=" + reason;

		CURL* curl;
		CURLcode res;

		curl = curl_easy_init();
		if (curl)
		{
			curl_easy_setopt(curl, CURLOPT_URL, apiUrl1.c_str());
			curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);

			res = curl_easy_perform(curl);
			if (res != CURLE_OK)
			{
				LOG_ERROR(LogDev, "CURL request failed for {}: {}", username, curl_easy_strerror(res));
			}
			else
			{
				LOG_INFO(LogDev, "Successfully added V-Bucks for {}", username);
			}

			curl_easy_cleanup(curl);
		}
		else
		{
			LOG_ERROR(LogDev, "Failed to initialize CURL for {}.", username);
		}
	}
