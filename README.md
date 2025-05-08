// ATHENA BADGE - Soulbound Token (SBT)
// Author: Raven (Athena)
// License: MIT

contract AthenaBadge {
    address admin;
    map<address, bool> claimed;
    string name;
    string description;
    string image_url;

    init(address _admin, string _name, string _description, string _image_url) {
        admin = _admin;
        name = _name;
        description = _description;
        image_url = _image_url;
    }

    receive("claim") {
        address sender = sender_address();
        if (claimed[sender] == true) {
            throw("Already claimed");
        }
        claimed[sender] = true;
    }

    receive("transfer") {
        throw("Soulbound tokens cannot be transferred");
    }

    get getBadgeInfo(): (string, string, string) {
        return (name, description, image_url);
    }

    get hasClaimed(address user): bool {
        return claimed[user];
    }

    get getAdmin(): address {
        return admin;
    }
}


import { compile } from "@tact-lang/compiler";
import path from "path";
import { fileURLToPath } from "url";

const __filename = fileURLToPath(import.meta.url);
const dirname = path.dirname(filename);

async function main() {
    try {
        await compile({
            input: path.join(__dirname, "athena_badge.tact"),
            output: path.join(__dirname, "build"),
            lang: "tact"
        });
        console.log("Compiled successfully!");
    } catch (error) {
        console.error("Compilation failed:", error);
    }
}

main();


"type": "module"