# Useful Note or Tricks

---

I’ve seen this on many occasions and users were confused why their container was being restarted. But if they’d used kubectl describe, they’d have seen that the container terminated with exit code 137 or 143, telling them that the pod was terminated externally. Additionally, the listing of the pod’s events would show that the container was killed because of a failed liveness probe. If you see this happening at pod startup, it’s because you failed to set initialDelaySeconds appropriately.

> NOTE Exit code 137 signals that the process was killed by an external signal (exit code is 128 + 9 (SIGKILL). Likewise, exit code 143 corresponds to 128 + 15 (SIGTERM).

---

If you’re running a Java app in your container, be sure to use an HTTP GET liveness probe instead of an Exec probe, where you spin up a whole new JVM to get the liveness information. The same goes for any JVM-based or sim- ilar applications, whose start-up procedure requires considerable computa- tional resources.
