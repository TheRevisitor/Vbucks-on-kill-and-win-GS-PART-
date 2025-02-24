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
