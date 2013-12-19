package dan.woodward.plugin;

import java.util.HashMap;
import java.util.Map;

import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerCommandPreprocessEvent;
import org.bukkit.event.player.PlayerJoinEvent;
import org.bukkit.event.player.PlayerToggleSprintEvent;
import org.bukkit.plugin.java.JavaPlugin;

public class RunFast extends JavaPlugin implements Listener {

	private final static float speed_slow = 0.03F;
	private final static float speed_normal = 0.2F;
	private final static float speed_fast = 0.3F;
	private final static float speed_faster = 0.4F;
	private final static float speed_fastest = 0.5F;
	private final static float speed_ludicrous = 0.999F;

	private final Map<Player, Float> playerSpeedCache = new HashMap<>();

	@SuppressWarnings("serial")
	private final Map<String, Float> speeds = new HashMap<String, Float>() {{
		put("slow", speed_slow);
		put("normal", speed_normal);
		put("fast", speed_fast);
		put("faster", speed_faster);
		put("fastest", speed_fastest);
		put("ludicrous", speed_ludicrous);
	}};

	@EventHandler
	public void onLogin(PlayerJoinEvent event) {
		Player player = event.getPlayer();
		player.setFlySpeed(speed_ludicrous);
	}

	@Override
	public void onEnable(){
		getServer().getPluginManager().registerEvents(this, this);
	}

	@EventHandler
	public void onCommand(PlayerCommandPreprocessEvent event) {
		String[] commandArgs = event.getMessage().substring(1).toLowerCase().split("\\s+");

		String generalUsageMsg = "usage: Run (slow|normal|fast|faster|fastest|ludicrous)";

		if ("run".equalsIgnoreCase(commandArgs[0])) {
			event.setCancelled(true);

			Player player = event.getPlayer();

			if (commandArgs.length == 1 || commandArgs.length > 2) {
				player.sendMessage(generalUsageMsg);
				return;
			}

			String command = commandArgs[1].toLowerCase();
			if (speeds.containsKey(command)) {
				Float speed = speeds.get(command);
				playerSpeedCache.put(player, speed);
				player.sendMessage("Your speed is set to: " + command + " (" + speed + ")");
			} else {
				player.sendMessage(command + " is not a valid speed setting\n" + generalUsageMsg);
			}
		}
	}

	@EventHandler
	public void onTobbleSprint(PlayerToggleSprintEvent event) {
		Player player = event.getPlayer();
		if (!playerSpeedCache.containsKey(player)) {
			playerSpeedCache.put(player, speed_ludicrous);
		}
		if (player.isSprinting()) {
			player.setWalkSpeed(speed_normal);
		} else {
			Float playerSpeed = playerSpeedCache.get(player);
			player.setWalkSpeed(playerSpeed);
		}
	}
}
